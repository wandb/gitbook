---
description: 如何设置集中式W＆B本地服务器
---

# Setup

W＆B Local Server是在基础架构上运行的docker映像。有关如何置备新实例的说明，请参阅以下内容。

## **亚马逊网络服务\(AWS\)**

可以通过几种不同的方式在AWS中运行wandb/local。

### AWS Fargate

在AWS控制台中输入Fargate，[或直接转到ECS管理页面](https://console.aws.amazon.com/ecs/home)。单击Get Started，使用Fargate创建新的ECS服务。

* **Container Definition**：选择“自定义custom”，然后单击“配置configure”。在这里将您的容器命名为wandb-local，并将映像名称设置为wandb/local:latest。最后，将端口8080添加到端口映射中。
* **Task Definition**：单击“编辑edit”，并确保为任务分配至少8GB的ram和4个vCPU
* **Define Your Service**：您可能想创建一个ALB，该ALB可以终止SSL并将请求转发到该服务的端口8080。
* **IAM Permissions**：如果您打算使用云文件后端（这是可选的），请确保您的实例具有IAM角色，以使其可以访问S3并订阅SQS。

  设置服务后，您可以通过ALB或直接通过实例的IP和PORT访问它。您的实例从启动即可使用，但是对于高级选项，您现在可以参考 [configuring your instance](../../self-hosted/configuration.md)。

### EC2

您可以在也安装了Docker的任何EC2实例上运行wandb / local。我们建议至少8GB的RAM和4vCPU。只需运行以下命令即可启动容器：

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **谷歌云**

#### Kubernetes引擎

如果您已经在运行k8s，则可以轻松地将wandb/local启动到现有集群中。 GCP使得通过[控制台](https://console.cloud.google.com/projectselector2/kubernetes/list?ref=https:%2F%2Fapp.gitbook.com%2F@weights-and-biases%2Fs%2Fdocs%2F~%2Fdrafts%2F-MKaPhwzNIegNuInaekR%2Fself-hosted-1%2Fsetup&pli=1&authuser=1&supportedpurview=project)启动集群变得很简单。

以下k8s yaml可以自定义，但应作为在GCP中使用负载平衡和SSL配置本地的基础。 以下yaml假定您已创建一个名为**wandb-local-static-ip**的静态IP地址。 您可以执行以下操作：

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

### 计算引擎

您可以在也安装了Docker的任何计算引擎实例上运行wandb / local。 我们建议至少8GB的RAM和4vCPU。 只需运行以下命令即可启动容器

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

### **Azure Kubernetes服务**

可以定制以下k8s yaml，但应将其用作配置本地的基础。

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

上面“ Azure Kubernetes服务”中的k8s YAML应该可以在大多数本地安装中使用

## OnPrem Docker

您可以在还安装了Docker的任何实例上运行wandb/local。 我们建议至少8GB的RAM和4vCPU。 只需运行以下命令即可启动容器：

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

