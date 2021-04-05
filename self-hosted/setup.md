---
description: 一元化されたW＆Bローカルサーバーをセットアップする方法
---

# Setup

W＆Bローカルサーバーは、インフラストラクチャで実行されているDockerイメージです。新しいインスタンスをプロビジョニングする方法については、以下を参照してください。

## アマゾンウェブサービス

AWSでの_wandb/local_の実行は、いくつかの異なる方法で実行できます。

### AWSファーゲート

 AWSコンソールにFargateと入力するか、[ECS管理ページ](https://console.aws.amazon.com/ecs/home)に直接移動します。\[開始\]をクリックして、Fargateを使用して新しいECSサービスを作成します。

* **コンテナ定義**：「カスタム」を選択し、「構成」をクリックします。ここから、コンテナにwandb-localという名前を付け、イメージ名を_wandb/local:latest_に設定します。最後に、ポート8080をポートマッピングに追加します。
* **タスク定義**：\[編集\]をクリックし、タスクに少なくとも8GBのRAMと4つのvCPUを指定します
* **サービス定義**：SSLを終了し、このサービスのポート8080に要求を転送できるALBを作成することをお勧めします。
* **IAMアクセス許可**：クラウドファイルバックエンドを使用する場合（これはオプションです）、インスタンスにS3へのアクセスとSQSへのサブスクライブを許可するIAMロールがあることを確認します。サービスがプロビジョニングされると、ALBを介して、またはインスタンスのIPとPORTを介して直接サービスにアクセスできます。インスタンスは起動時に使用できますが、詳細オプションについては、インスタンスの構成に進むことができます。

サービスがプロビジョニングされると、ALBを介して、またはインスタンスのIPとPORTを介して直接サービスにアクセスできます。インスタンスは起動時に使用できますが、詳細オプションについては、[インスタンスの構成](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MNdlQobOrN8f63KfkRZ/v/japanese/self-hosted/configuration)に進むことができます。

### EC2Docker

がインストールされているEC2インスタンスで_wandb/local_を実行できます。少なくとも8GBのRAMと4vCPUをお勧めします。次のコマンドを実行して、コンテナを起動します。

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## グーグル・クラウド

Kubernetesエンジン

すでにk8sを実行している場合は、既存のクラスターで_wandb/local_を簡単に起動できます。GCPを使用すると、コンソールからクラスタを簡単に起動できます。

次のk8s yamlはカスタマイズできますが、GCPで負荷分散とSSLを使用してローカルを構成するための基本的な基盤として機能する必要があります。以下のyamlは、**wandb-local-static-ip**という名前の静的IPアドレスを作成したことを前提としています。あなたは次のものと共にそれが可能です。

```text
gcloud compute addresses create wandb-local-static-ip --global
```

```bash
spec:
  strategy:
    type: Recreate
  replicas: 1
  selector:
    matchLabels:
      app: wandb
  template:
    metadata:
      labels:
        app: wandb
    spec:
      volumes:
        - name: wandb-persistent-storage
          persistentVolumeClaim:
            claimName: wandb-pv-claim
          name: wandb
      imagePullSecrets:
        - name: github
      containers:
        - name: wandb
          imagePullPolicy: Always
          image: wandb/local
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          volumeMounts:
            - name: wandb
              mountPath: /vol
          livenessProbe:
            httpGet:
              path: /healthz
              port: http
          readinessProbe:
            httpGet:
              path: /ready
              port: http
          resources:
            requests:
              cpu: "1500m"
              memory: 4G
            limits:
              cpu: "4000m"
              memory: 8G
---
apiVersion: v1
kind: Service
metadata:
  name: wandb-service
spec:
  type: NodePort
  selector:
    app: wandb
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: wandb-ingress
  annotations:
    kubernetes.io/ingress.global-static-ip-name: wandb-local-static-ip
    networking.gke.io/managed-certificates: wandb-local-cert
spec:
  backend:
    serviceName: wandb-service
    servicePort: 80
---
apiVersion: networking.gke.io/v1beta1
kind: ManagedCertificate
metadata:
  name: wandb-local-cert
spec:
  domains:
    - REPLACE_ME
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wandb-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Gi
```

### Compute Engine

You can run _wandb/local_ on any Compute Engine instance that also has Docker installed. We suggest at least 8GB of RAM and 4vCPU's. Simply run the following command to launch the container:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Azure Kubernetesサービス

次のk8s yamlはカスタマイズできますが、ローカルを構成するための基本的な基盤として機能する必要があります。

```text
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wandb
  labels:
    app: wandb
spec:
  strategy:
    type: Recreate
  replicas: 1
  selector:
    matchLabels:
      app: wandb
  template:
    metadata:
      labels:
        app: wandb
    spec:
      volumes:
        - name: wandb-persistent-storage
          persistentVolumeClaim:
            claimName: wandb-pv-claim
          name: wandb
      containers:
        - name: wandb
          imagePullPolicy: IfNotPresent
          image: wandb/local:latest
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          volumeMounts:
            - name: wandb
              mountPath: /vol
          livenessProbe:
            httpGet:
              path: /healthz
              port: http
          readinessProbe:
            httpGet:
              path: /ready
              port: http
          resources:
            requests:
              cpu: "2000m"
              memory: 4G
            limits:
              cpu: "4000m"
              memory: 8G
---
apiVersion: v1
kind: Service
metadata:
  name: wandb-service
spec:
  type: NodePort
  selector:
    app: wandb
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: wandb-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  backend:
    serviceName: wandb-service
    servicePort: 80
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wandb-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Gi
```

## OnPrem Kubernetes

上記のAzure Kubernetesサービスセクションのk8s YAMLは、ほとんどのオンプレミスインストールで機能します。

## OnPrem Docker

Dockerがインストールされている任意のインスタンスで_wandb/local_を実行できます。少なくとも8GBのRAMと4vCPUをお勧めします。次のコマンドを実行して、コンテナを起動します。

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

