---
description: How to set up a centralized W&B Local Server
---

# Setup

A W&B Local Server is a docker image running on your infrastructure. See the following for instructions for how to provision a new instance.

## Amazon Web Services

Running _wandb/local_ in AWS can be done in a few different ways.

### AWS Fargate

Type Fargate in the AWS console, or go directly to the [ECS management page](https://console.aws.amazon.com/ecs/home). Click Get Started to create a new ECS service using Fargate.

* **Container Definition**: Choose "custom" and click "Configure".  From here name your container _wandb-local_ and set the image name to _wandb/local:latest_.  Finally add port 8080 to the port mappings.
* **Task Definition**: Click Edit and make sure to give the task at least 8GB of ram and 4 vCPUs
* **Define Your Service**: You'll likely want to create an ALB that can terminate SSL and forward requests to port 8080 of this service.
* **IAM Permissions**: If you plan to use a cloud file backend \(this is optional\), make sure your instance has an IAM role that allows it to access S3 and subscribe to SQS.

Once the service is provisioned you can access it via your ALB or directly via the IP and PORT of your instance. Your instance is usable from boot, but for advanced options, you may now proceed to [configuring your instance](configuration.md).

### EC2

You can run _wandb/local_ on any EC2 instance that also has Docker installed. We suggest at least 8GB of RAM and 4vCPU's. Simply run the following command to launch the container:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Google Cloud Platform

### Kubernetes Engine

If you're running k8s already you can easily launch _wandb/local_ into an existing cluster. GCP always make it really simple to launch a cluster via the [console](https://console.cloud.google.com/kubernetes/list).

The following k8s yaml can be customized but should serve as a basic foundation for configuring local with load balancing and SSL in GCP. The yaml below assumes you've created a static IP address named **wandb-local-static-ip**. You can do so with:

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

## Azure Kubernetes Service

The following k8s yaml can be customized but should serve as a basic foundation for configuring local.

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

The k8s YAML above in the Azure Kubernetes Service section should work in most on-premise installations.

## OnPrem Docker

You can run _wandb/local_ on any instance that also has Docker installed. We suggest at least 8GB of RAM and 4vCPU's. Simply run the following command to launch the container:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

