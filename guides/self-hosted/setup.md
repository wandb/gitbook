---
description: How to set up a centralized W&B Local Server
---

# Production Setup

A W&B Local Server is a Docker container running on your infrastructure that connects to scalable data stores. See the following for instructions for how to provision a new instance.

{% hint style="info" %}
Check out a [video tutorial](https://www.youtube.com/watch?v=bYmLY5fT2oA) for getting set up using [Terraform](https://www.terraform.io/) on AWS!
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=bYmLY5fT2oA" %}

## Amazon Web Services

The simplest way to configure W&B within AWS is to use our [official Terraform](https://github.com/wandb/local/tree/main/terraform/aws).  Detailed instructions can be found in the README.  If instead you want to configure services manually you can find [instructions here](configuration.md#amazon-web-services).

## Microsoft Azure

The simplest way to configure W&B within Azure is to use our [official Terraform](https://github.com/wandb/local/tree/main/terraform/azure).  Detailed instructions can be found in the README.  If instead you want to configure services manually you can find [instructions here](configuration.md#azure).

## Google Cloud Platform

These instructions assume you have a GKE k8s cluster already configured. They also assume you have `gcloud` command via the [Cloud SDK](https://cloud.google.com/sdk) and docker running locally. This will run the W&B service on the internet. If instead you only want this service running on your internal private network, you'll need to modify the instructions to use a Service of type LoadBalancer with the appropriate annotations and SSL configuration as described [here](https://pushbuildtestdeploy.com/how-to-run-an-internal-load-balancer-with-ssl-on-gke/).

### Credentials

Create a Service account in the cloud console IAM with the following roles:

* Service Account Token Creator
* Pub/Sub Admin
* Storage Object Admin
* Cloud SQL Client

{% hint style="info" %}
You can optionally restrict Pub/Sub and Cloud Storage roles to only apply to the subscriptions / bucket we create in future steps
{% endhint %}

Download a key in JSON format then create the following secret in your kubernetes cluster:

```text
kubectl create secret generic wandb-service-account --from-file=key.json=PATH-TO-KEY-FILE.json
```

### Storage

Create a bucket in the same region as your k8s cluster.

Navigate to **Pub/Sub** &gt; **Topics** in the GCP Console, and click "**Create topic**". Choose a name and create a topic.

Navigate to **Pub/Sub** &gt; **Subscriptions** in the GCP Console, and click "**Create subscription**". Choose a name, and make sure Delivery Type is set to "Pull".  Click "**Create**".

Finally make sure `gcloud` is configured with the project you're using and run:

```text
gsutil notification create -t TOPIC_NAME -f json gs://BUCKET_NAME
```

Write the following to a file named `cors.json`

```text
[
  {
    "origin": [
      "*"
    ],
    "responseHeader": ["Content-Type", "x-goog-acl"],
    "method": ["GET", "HEAD", "PUT"],
    "maxAgeSeconds": 3600
  }
]
```

Then run:

```text
gsutil cors set cors.json gs://BUCKET_NAME
```

### Setup MySQL

Provision a Cloud SQL instance with version 5.7 of MySQL in the same region as your other resources. Note the **Connection name** once the instance has been provisions and replace YOUR\_MYSQL\_CONNECTION\_NAME in the templates below with it.

Connect to your new database:

```text
gcloud sql connect YOUR_DATABASE_NAME --user=root --quiet
```

Then run the following SQL:

```sql
CREATE USER 'wandb_local'@'%' IDENTIFIED BY 'wandb_local';
CREATE DATABASE wandb_local CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
GRANT ALL ON wandb_local.* TO 'wandb_local'@'%' WITH GRANT OPTION;
```

### Create a k8s deployment

This assumes you've created a static IP and are using GKE to manage certificates.

#### Create global static IP

```text
gcloud compute addresses create wandb-local-static-ip --global
```

You'll want to configure a DNS entry that points to this IP once it's been created, referred to as YOUR\_DNS\_NAME below.

#### Create k8s deployment with a Google managed certificate

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wandb
  labels:
    app: wandb
spec:
  strategy:
    type: RollingUpdate
  replicas: 1
  selector:
    matchLabels:
      app: wandb
  template:
    metadata:
      labels:
        app: wandb
    spec:
      containers:
        - name: cloud-sql-proxy
          # It is recommended to use the latest version of the Cloud SQL proxy
          # Make sure to update on a regular schedule!
          image: gcr.io/cloudsql-docker/gce-proxy:1.20.2
          command:
            - "/cloud_sql_proxy"
            # If connecting from a VPC-native GKE cluster, you can use the
            # following flag to have the proxy connect over private IP
            # - "-ip_address_types=PRIVATE"
            - "-instances=YOUR_MYSQL_CONNECTION_NAME=tcp:3306"
            - "-credential_file=/secrets/key.json"
          securityContext:
            # The default Cloud SQL proxy image runs as the
            # "nonroot" user and group (uid: 65532) by default.
            runAsNonRoot: true
          volumeMounts:
            - name: wandb-service-account
              mountPath: /secrets/
              readOnly: true
          resources:
            requests:
              cpu: 100m
              memory: 32Mi
            limits:
              cpu: 1000m
              memory: 512Mi
        - name: wandb
          env:
            - name: BUCKET
              value: gs://YOUR_BUCKET_NAME
            - name: BUCKET_QUEUE
              value: pubsub:/PROJECT_NAME/TOPIC_NAME/SUBSCRIPTION_NAME
            - name: GOOGLE_APPLICATION_CREDENTIALS
              value: /var/secrets/google/key.json
            - name: MYSQL
              value: mysql://wandb_local:wandb_local@127.0.0.1:3306/wandb_local
            - name: LICENSE
              value: $REPLACE_ME_WITH_YOUR_LICENSE
          imagePullPolicy: Always
          image: wandb/local:latest
          command:
            - sh
            - -c
            - "sleep 10 && /sbin/my_init"
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          volumeMounts:
            - name: wandb-service-account
              mountPath: /var/secrets/google
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
      volumes:
        - name: wandb-service-account
          secret:
            secretName: wandb-service-account
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
  defaultBackend:
    service:
      name: wandb-service
      port:
        number: 80
---
apiVersion: networking.gke.io/v1beta1
kind: ManagedCertificate
metadata:
  name: wandb-local-cert
spec:
  domains:
    - YOUR_DNS_DOMAIN
```

### Configure your instance

Go to https://YOUR\_DNS\_DOMAIN in your browser, and create your account. Choose "System Settings" from the menu in the upper right.

Input the LICENSE we've provided and click "Use and external file storage backend". Specify: `pubsub:/PROJECT/TOPIC/SUBSCRIPTION` in the **Notification Subscription** section.

Input https://YOUR\_DNS\_DOMAIN in the **Frontend Host** section. Click update settings.

### Verify your installation

On a machine with python run:

```bash
pip install wandb
wandb login --host=https://YOUR_DNS_DOMAIN
wandb verify
```

## On Premise / Baremetal

W&B depends on scalable data stores that must be configured and managed by your operations team.  The team must provide a MySQL 5.7 database server and an S3 compatible object store for the application to scale properly.

### MySQL 5.7

{% hint style="warning" %}
W&B only supports MySQL 5.7 we do not support MySQL 8 or any other SQL engine.
{% endhint %}

There are a number of enterprise services that make operating a scalable MySQL database simpler.  We suggest looking into one of the following solutions:

* [https://www.percona.com/software/mysql-database/percona-server](https://www.percona.com/software/mysql-database/percona-server)
* [https://github.com/mysql/mysql-operator](https://github.com/mysql/mysql-operator)

The most important things to consider when running your own MySQL 5.7 database are:

1. **Backups**. You should be periodically backing up the database to a separate facility.  We suggest daily backups with at least 1 week of retention.
2. **Performance.** The disk the server is running on should be fast.  We suggest running the database on an SSD or accelerated NAS.
3. **Monitoring.** The database should be monitored for load.  If CPU usage is sustained at &gt; 40% of the system for more than 5 minutes it's likely a good indication the server is resource starved.
4. **Availability.** Depending on your availability and durability requirements you may want to configure a hot standby on a separate machine that streams all updates in realtime from the primary server and can be used to failover to incase the primary server crashes or become corrupted.

Once you've provisioned a MySQL 5.7 database you can create a database and user using the following SQL \(replacing SOME\_PASSWORD\).

```sql
CREATE USER 'wandb_local'@'%' IDENTIFIED BY 'SOME_PASSWORD';
CREATE DATABASE wandb_local CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
GRANT ALL ON wandb_local.* TO 'wandb_local'@'%' WITH GRANT OPTION;
```

### Object Store

The S3 compatible object store can be an externally hosted [Minio cluster](https://docs.min.io/minio/k8s/), or W&B supports any S3 compatible object store that has support for signed urls.  To see if your object store supports signed urls, you can run the [following script](https://gist.github.com/vanpelt/2e018f7313dabf7cca15ad66c2dd9c5b).  When connecting to an s3 compatible object store you can specify your credentials in the connection string, i.e.  

```yaml
s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME
```

By default we assume 3rd party object stores are not running over HTTPS.  If you've configured a trusted SSL certificate for your object store, you can tell us to only connect over tls by adding the `tls` query parameter to the url, i.e.

{% hint style="warning" %}
This will only work if the SSL certificate is trusted.  We do not support self signed certificates.
{% endhint %}

```yaml
s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME?tls=true
```

When using 3rd party object stores, you'll want to set `BUCKET_QUEUE` to `internal://`.  This tells the W&B server to manage all object notification internally instead of depending on SQS.

The most important things to consider when running your own Object store are:

1. **Storage capacity and performance**.  It's fine to use magnetic disks, but you should be monitoring the capacity of these disks.  Average W&B usage results in 10's to 100's of Gigabytes.  Heavy usage could result in Petabytes of storage consumption.
2. **Fault tolerance.**  At a minimum the physical disk storing the objects should be on a RAID array.  Consider running Minio in [distributed mode](https://docs.min.io/minio/baremetal/installation/deploy-minio-distributed.html#deploy-minio-distributed).
3. **Availability.**  Monitoring should be configured to ensure the storage is available.

There are many enterprise alternatives to running your own object storage service such as:

1. [https://aws.amazon.com/s3/outposts/](https://aws.amazon.com/s3/outposts/)
2. [https://www.netapp.com/data-storage/storagegrid/](https://www.netapp.com/data-storage/storagegrid/)

#### Minio setup

If you're using minio, you can run the following commands to create a bucket .

```bash
mc config host add local http://$MINIO_HOST:$MINIO_PORT "$MINIO_ACCESS_KEY" "$MINIO_SECRET_KEY" --api s3v4
mc mb --region=us-east1 local/local-files
```

### Kubernetes Deployment

The following k8s yaml can be customized but should serve as a basic foundation for configuring local in kubernetes.  

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wandb
  labels:
    app: wandb
spec:
  strategy:
    type: RollingUpdate
  replicas: 1
  selector:
    matchLabels:
      app: wandb
  template:
    metadata:
      labels:
        app: wandb
    spec:
      containers:
        - name: wandb
          env:
            - name: LICENSE
              value: XXXXXXXXXXXXXXX
            - name: BUCKET
              value: s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME
            - name: BUCKET_QUEUE
              value: internal://
            - name: AWS_REGION
              value: us-east1
            - name: MYSQL
              value: mysql://$USERNAME:$PASSWORD@$HOSTNAME/$DATABASE
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
```

The k8s YAML above should work in most on-premise installations.  However the details of your Ingress and optional SSL termination will vary.  See [networking](setup.md#networking) below.

### Openshift

W&B supports operating from within an [Openshift kubernetes cluster](https://www.redhat.com/en/technologies/cloud-computing/openshift).  Simply follow the instructions in the kubernetes deployment section above.

### Docker

You can run _wandb/local_ on any instance that also has Docker installed. We suggest at least 8GB of RAM and 4vCPU's. Simply run the following command to launch the container:

```bash
 docker run --rm -d \
   -e LICENSE=XXXXX \
   -e BUCKET=s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME \
   -e BUCKET_QUEUE=internal:// \
   -e AWS_REGION=us-east1 \
   -e MYSQL=mysql://$USERNAME:$PASSWORD@$HOSTNAME/$DATABASE \
   -p 8080:8080 --name wandb-local wandb/local
```

{% hint style="warning" %}
You'll want to configure a process manager to ensure this process is restarted if it crashes.  A good overview of using SystemD to do this can be [found here](https://blog.container-solutions.com/running-docker-containers-with-systemd).
{% endhint %}

## Networking

### Load Balancer

You'll want to run a load balancer that terminates network requests at the appropriate network boundary.  Some customers expose their wandb service on the internet, others only expose it on an internal VPN/VPC.  It's important that both the machines being used to execute machine learning payloads and the devices users access the service through web browsers can communicate to this endpoint.  Common load balancers include:

1. [Nginx Ingress](https://kubernetes.github.io/ingress-nginx/)
2. [Istio](https://istio.io/)
3. [Caddy](https://caddyserver.com/)
4. [Cloudflare](https://www.cloudflare.com/load-balancing/)
5. [Apache](https://www.apache.org/)
6. [HAProxy](http://www.haproxy.org/)

### SSL / TLS

The W&B application server does not terminate SSL.  If your security policies require SSL communication within your trusted networks consider using a tool like Istio and [side car containers](https://istio.io/latest/docs/reference/config/networking/sidecar/).  The load balancer itself should terminate SSL with a valid certificate.  Using self-signed certificates is not supported and will cause a number of challenges for users.  If possible using a service like [Let's Encrypt](https://letsencrypt.org/) is a great way to provided trusted certificates to your load balancer.  Services like Caddy and Cloudflare manage SSL for you.

## Verifying your installation

Regardless of how your server was installed, it's a good idea everything is configured properly.  W&B makes it easy to verify everything is properly configured by using our CLI. 

```bash
pip install wandb
wandb login --host=https://YOUR_DNS_DOMAIN
wandb verify
```

If you see any errors contact W&B support staff.  You can also see any errors the application hit at startup by checking the logs.

### Docker

```bash
docker logs wandb-local
```

### Kubernetes

```bash
kubectl get pods
kubectl logs wandb-XXXXX-XXXXX
```

