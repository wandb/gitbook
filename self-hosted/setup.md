---
description: Comment mettre en place un Serveur W&B Local centralisé
---

# Setup

Un Serveur W&B Local est une image docker qui s’exécute sur votre infrastructure. Consultez les articles suivants pour obtenir des instructions sur la manière d’implanter une nouvelle instance.

## Amazon Web Services

Exécuter wandb/local dans AWS peut être effectué de différentes manières.

### AWS Fargate

Inscrivez Fargate dans la console AWS, ou rendez-vous directement sur la [page de gestion ECS](https://console.aws.amazon.com/ecs/home). Cliquez sur Commencer \(Get Started\) pour créer un nouveau service ECS qui utilise Fargate.

*  **Définition de Container :** Choisissez "personnalisée" \(custom\) et cliquez sur "Configurer" \(Configure\). De là, nommez votre container wandb-local et réglez le nom de l’image sur wandb/local:latest. Enfin, ajoutez le port 8080 à la redirection de ports \(port mappings\).
*   **Définition de Tâche :** Cliquez sur Éditer \(Edit\) et assurez-vous de donner au moins 8GB de ram et 4 vCPUS à la tâche
* **Définissez votre service :** Vous voudrez sûrement créer un ALB qui peut terminer du SSL et faire passer des requêtes au port 8080 de ce service.
* **Autorisations IAM :** Si vous prévoyez d’utiliser une backend de fichier de cloud \(ce qui est optionnel\), assurez-vous que votre instance à un rôle IAM qui lui permet d’accéder à S3 et de s’inscrire à SQS.

Une fois que le service est approvisionné, vous pouvez y accéder via votre ALB ou directement par l’IP et le PORT de votre instance. Votre instance est utilisable à partir du boot, mais pour les options avancées, vous pouvez désormais [configurer votre instance](https://docs.wandb.ai/self-hosted/configuration). 

### EC2

Vous pouvez exécuter wandb/local sur n’importe quelle instance EC2 sur laquelle Docker est aussi installée. Nous vous suggérons au moins 8 GB de RAM et 4vCPU. Exécutez simplement la commande suivante pour lancer le container :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Google Cloud Platform

### Kubernetes Engine

Si vous utilisez déjà k8s, vous pouvez facilement lancer wandb/local dans un cluster existant. GCP rend toujours le lancement d’un cluster très simple via la [console](https://console.cloud.google.com/kubernetes/list).

Le yaml k8s suivant peut être personnalisé, mais devrait servir de fondation de base pour configurer local avec une répartition de charge \(load balancing\) et SSL dans GCP. Le yaml ci-dessous suppose que vous avez créé une adresse IP statique appelée **wandb-local-static-ip**. Vous pouvez faire ceci avec :

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

Vous pouvez exécuter wandb/local sur n’importe quelle instance Compute Engine sur laquelle Docker est aussi installé. Nous vous suggérons au moins 8 GB de RAM et 4vCPU. Exécutez simplement la commande suivante pour lancer le container :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Azure Kubernetes Service

Le yaml k8s suivant peut être personnalisé, mais devrait servir de fondation de base pour configurer local.

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

Le YAML k8s ci-dessus dans la section Azure Kubernetes Service devrait fonctionner pour la plupart des installations sur site \(on-premise\).

## OnPrem Docker

Vous pouvez exécuter wandb/local sur n’importe quelle instance qui a déjà Docker d’installé. Nous vous suggérons au moins 8 GB de RAM et 4vCPU. Exécutez simplement la commande suivante pour lancer le container :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

