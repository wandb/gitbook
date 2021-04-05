---
description: 중앙 집중식 W&B 로컬 서버 설정 방법
---

# Setup

 W&B 로컬 서버는 여러분의 인프라\(infrastructure\)에서 실행되는 도커 이미지\(docker image\)입니다. 새 인스턴스를 프로비저닝\(provision\)하는 방법은 다음을 참고하시기 바랍니다.

##  **아마존 웹 서비스**

wandb/local를 AWS\(아마존 웹 서비스\)에서 실행하는 방법에는 몇 가지가 있습니다.

### **AWS Fargate\(파게이트\)**

AWS 콘솔에 Fargate를 입력하시거나 [ECS management 페이지](https://console.aws.amazon.com/ecs/home)로 직접 이동합니다. Get Started\(시작하기\)를 클릭하여 Fargate를 사용하는 새 ECS 서비스를 생성합니다.

* **Container Definition\(컨테이너 정의\)**: “custom\(사용자 정의\)”를 선택하고 “Configure\(구성\)”을 클릭합니다. 여기서 컨테이너의 이름을 wandb-local으로 지정하고 이미지 이름을 wandb/local:latest으로 설정합니다. 마지막으로 port\(포트\) 8080을 port mapping\(포트 매핑\)에 추가합니다.
* **Task Definition\(작업 정의\):** Edit\(편집\)를 클릭하고 작업에 최소 8GB의 램과 4 vCPU를 할당하셔야 합니다
* **Define Your Service\(서비스 정의하기\)**: SSL을 종료하고 요청을 port 8080으로 전달할 수 있는 ALB를 생성하실 수 있습니다.
* **IAM Permissions\(IAM 권한\)**: 클라우드 파일 백엔드\(선택사항\)를 사용하실 예정이시라면, 인스턴스\(instance\)에 S3에 액세스하고 SQS를 구독을 허용하는 IAM role이 필요합니다.

 일단 서비스가 프로비저닝\(provision\) 되면, ALB 또는 인스턴스의 IP 및 PORT를 통해 직접 액세스하실 수 있습니다. 부팅 시 인스턴스를 사용하실 수 있지만, 고급 옵션의 경우, 이제 [configuring your instance\(인스턴스 구성\)](https://docs.wandb.com/self-hosted/configuration)를 계속해서 하실 수 있습니다.

### EC2

도커\(Docker\)가 설치된 모든 EC2 인스턴스에서 wandb/local을 실행하실 수 있습니다. 최소 8GB 램과 4vCPU를 권장합니다. 컨테이너를 시작하시려면 다음의 명령을 실행하시면 됩니다:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

##  **Google 클라우드 플랫폼**

###  **Kubernetes 엔진**

이미 k8s을 실행 중인 경우, wandb/local을 기존 클러스터\(cluster\)에 쉽고 간단하게 실행하실 수 있습니다. GCP를 사용하시면 언제나 [콘솔](https://console.cloud.google.com/kubernetes/list)을 통해 클러스터를 쉽고 간단하게 진행할 수 있습니다.

다음 k8s yaml을 사용자 지정하실 수 있지만, 이는 GCP에서 로드 밸런싱\(load balancing\) 및 SSL을 통해 로컬을 구성하는 기본 토대의 역할을 하여야 합니다. 아래의 yaml은 여러분께서 wandb-local-static-ip라는 정적\(static\) IP 주소를 생성했다고 가정합니다. 다음을 통해 이를 수행하실 수 있습니다:

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

###  **컴퓨트 엔진 \(Compute Engine\)**

도커\(Docker\)가 설치된 모든 컴퓨트 엔진 인스턴스\(Compute Engine instance\)에서 wandb/local을 실행하실 수 있습니다. 최소 8GB 램과 4vCPU를 권장합니다. 컨테이너를 시작하시려면 다음의 명령을 실행하시면 됩니다:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## **Azure Kubernetes 서비스**

다음 k8s yaml을 사용자 지정하실 수 있지만, 이는 로컬을 구성하는 기본 토대의 역할을 하여야 합니다.

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

## **온프렘 Kubernetes**

Azure Kubernetes 서비스 섹션에서 위의 k8s YAML은 대부분의 온프레미스\(on-premise\) 설치에서 작동해야 합니다.

##  **온프렘 도커\(Doker\)**

도커\(Docker\)가 설치된 모든 인스턴스에서 wandb/local을 실행하실 수 있습니다. 최소 8GB 램과 4vCPU를 권장합니다. 컨테이너를 시작하시려면 다음의 명령을 실행하시면 됩니다:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

