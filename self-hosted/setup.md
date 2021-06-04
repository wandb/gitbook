---
description: Comment mettre en place un serveur W&B Local centralisé
---

# Setup

Un serveur W&B Local est une image Docker qui s’exécute sur votre infrastructure. Consultez les instructions suivantes pour l’implantation d’une nouvelle instance.

## Amazon Web Services

Exécuter wandb/local sur AWS peut être effectué de différentes manières.

### AWS Fargate

Saisissez Fargate sur la console AWS, ou rendez-vous directement sur la [page de gestion ECS](https://console.aws.amazon.com/ecs/home). Cliquez sur Démarrer \(Get Started\) pour créer un nouveau service ECS utilisant Fargate.

*  **Définition du conteneur :** choisissez Personnalisée \(custom\) et cliquez sur Configurer \(Configure\). De là, nommez votre conteneur wandb-local et paramétrez le nom de l’image sur wandb/local:latest. Enfin, ajoutez le port 8080 au mappage des ports \(port mappings\).
* **Définition de tâche :** cliquez sur Éditer \(Edit\) et assurez-vous de fournir au moins 8 Go de RAM et 4 vCPUS à la tâche.
* **Définissez votre service :** vous devrez probablement créer un ALB capable d’effectuer une terminaison SSL et de transférer les requêtes au port 8080 de ce service.
* **Autorisations IAM :** si vous prévoyez d’utiliser un backend de fichier cloud \(ce qui est optionnel\), assurez-vous que votre instance ait un rôle dans la gestion des identités et des accès \(IAM\) qui lui permette d’accéder à S3 et de s’inscrire à SQS.

 Une fois que le service est fourni, vous pouvez y accéder via votre ALB ou directement par l’IP et le PORT de votre instance. Votre instance est utilisable au démarrage, mais pour les options avancées, vous pouvez désormais [configurer votre instance](https://docs.wandb.ai/v/fr/self-hosted/configuration).

### EC2

Vous pouvez également exécuter wandb/local sur n’importe quelle instance EC2 sur laquelle Docker est installée. Nous vous suggérons d’avoir au moins 8 Go de RAM et 4 vCPU. Il suffit de configurer la commande suivante pour lancer le conteneur :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## **Google Cloud Plateform \(GCP\)**

### **Kubernetes Engine \(GKE\)**

Si vous utilisez déjà k8s, vous pouvez facilement lancer wandb/local dans un cluster existant. GCP simplifie systématiquement le lancement d’un cluster via la [console](https://console.cloud.google.com/kubernetes/list).

Le fichier YAML K8S suivant peut être personnalisé, mais devrait servir de fondation de base pour une configuration locale avec un équilibrage des charges \(load balancing\) et un SSL dans GCP. Le YAML ci-dessous suppose que vous avez créé une adresse IP statique nommée **wandb-local-static-ip**. Vous pouvez le faire avec ce code :

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

### **Compute Engine \(GCE\)**

Vous pouvez exécuter wandb/local sur n’importe quelle instance Compute Engine sur laquelle Docker est aussi installé. Nous vous suggérons au moins 8 GB de RAM et 4vCPU. Exécutez simplement la commande suivante pour lancer le container :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Azure Kubernetes Service

Le fichier YAML K8S suivant peut être personnalisé, mais devrait servir de fondation de base pour une configuration locale.

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

Le fichier YAML K8S mentionné ci-dessus dans la section Azure Kubernetes Service devrait fonctionner pour la plupart des installations locales \(on-premise\).

## OnPrem Docker

Vous pouvez également exécuter wandb/local sur n’importe quelle instance sur laquelle Docker est déjà installé. Nous vous suggérons d’avoir au moins 8 Go de RAM et 4 vCPU. . Il suffit de configurer la commande suivante pour lancer le conteneur  :

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

