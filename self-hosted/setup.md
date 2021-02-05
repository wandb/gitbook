---
description: Cómo establecer un Servidor Local centralizado de W&B
---

# Setup

Un Servidor Local de W&B es una imagen de docker que se ejecuta en tu infraestructura. Mira las siguientes instrucciones de cómo suministrar una nueva instancia.

##  Servicios Web de Amazon

Ejecutar wandb/local en AWS se puede hacer de diferentes formas.

### AWS Fargate

Escribe Fargate en la consola de AWS, o ve directamente a la [página de administración de ECS](https://console.aws.amazon.com/ecs/home). Haz click en Empezar para crear un nuevo servicio ECS utilizando Fargate.

* **Definición del Contenedor**: Elige “personalizado” y haz click en “Configurar”. Desde aquí, nombra a tu contenedor wandb-local, y establece el nombre de la imagen a wandb/local:latest. Finalmente, agrega el puerto 8080 para los mapeos del puerto.
* **Definición de la Tarea:** Haz click en Editar y asegúrate de darle a la tarea al menos 8GB de ram y 4 vCPUs.
* **Define Tu Servicio:** Probablemente, quieras crear un ALB que pueda terminar SSL y reenviar las solicitudes al puerto 8080 de este servicio.
* **Permisos IAM:** Si planeas utilizar un backend para los archivos de la nube \(esto es opcional\), asegúrate de que tu instancia tenga un rol IAM que te permita acceder a S3 y suscribirte a SQS.

Una vez que el servicio sea provisto, puedes acceder al mismo a través de tu ALB, o directamente a través de la IP y el PORT de tu instancia. Tu instancia es utilizable desde el arranque, pero para establecer opciones avanzadas, ahora podrías proceder a [configurar tu instancia](https://docs.wandb.ai/self-hosted/configuration).

### EC2

Puedes ejecutar wandb/local en cualquier instancia EC2 que también tenga instalado a Docker. Te sugerimos al menos 8GB de RAM y 4 vCPUs. Simplemente, ejecuta el siguiente comando para lanzar el contenedor:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Plataforma de Google Cloud

### Motor de Kubernetes

Si ya estás corriendo k8s, puedes lanzar fácilmente wandb/local en un cluster existente. GCP siempre hace que lanzar un cluster a través de la [consola](https://console.cloud.google.com/kubernetes/list) sea realmente simple.

 El siguiente yaml de k8s puede ser personalizado, pero debería servir como un fundamento básico para configurar tu entorno local con balanceo de cargas y SSL en GCP. El yaml de abajo asume que has creado una dirección IP estática, llamada wandb-local-static-ip. Puedes hacerlo con:

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

Puedes ejecutar wandb/local en cualquier instancia de Compute Engine que también tenga instalado a Docker. Te sugerimos al menos 8GB de RAM y 4vCPUs. Simplemente, corre el siguiente comando para lanzar el contenedor:

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

## Servicio Kubernetes de Azure

  
El siguiente yaml de k8s puede ser personalizado, pero debería servir como un fundamento básico para la configuración local.

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

## Kubernetes de OnPrem

El YAML de k8s anterior, en la sección Servicio Kubernetes de Azure, debería funcionar en la mayoría de las instalaciones de los entornos locales..

## Docker de OnPrem

 Puedes correr wandb/local en cualquier instancia que también tenga instalado a Docker. Te sugerimos al menos 8GB de RAM y 4 vCPUs. Simplemente, ejecuta el siguiente comando para lanzar el contenedor.

```text
 docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

