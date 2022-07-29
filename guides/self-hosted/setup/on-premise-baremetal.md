---
description: Hosting W&B Server on baremetal servers on-premises
---

# On Prem / Baremetal

Run your bare metal infrastructure that connects to scaleable external data stores with W\&B Server. See the following for instructions on how to provision a new instance and guidance on provisioning external data stores.

{% hint style="warning" %}
W\&B application performance depends on scalable data stores that your operations team must configure and manage. The team must provide a MySQL 5.7 or MySQL 8 database server and an S3 compatible object store for the application to scale properly.
{% endhint %}

Talk to our sales team by reaching out to [contact@wandb.com](mailto:contact@wandb.com).

### MySQL 5.7

{% hint style="warning" %}
W\&B currently supports MySQL 5.7 or MySQL 8.
{% endhint %}

There are a number of enterprise services that make operating a scalable MySQL database simpler. We suggest looking into one of the following solutions:

* [https://www.percona.com/software/mysql-database/percona-server](https://www.percona.com/software/mysql-database/percona-server)
* [https://github.com/mysql/mysql-operator](https://github.com/mysql/mysql-operator)

The most important things to consider when running your own MySQL database are:

1. **Backups**. You should be periodically backing up the database to a separate facility. We suggest daily backups with at least 1 week of retention.
2. **Performance.** The disk the server is running on should be fast. We suggest running the database on an SSD or accelerated NAS.
3. **Monitoring.** The database should be monitored for load. If CPU usage is sustained at > 40% of the system for more than 5 minutes it's likely a good indication the server is resource starved.
4. **Availability.** Depending on your availability and durability requirements you may want to configure a hot standby on a separate machine that streams all updates in realtime from the primary server and can be used to failover to incase the primary server crashes or become corrupted.

Once you've provisioned a compatible MySQL database you can create a database and user using the following SQL (replacing SOME\_PASSWORD).

```sql
CREATE USER 'wandb_local'@'%' IDENTIFIED BY 'SOME_PASSWORD';
CREATE DATABASE wandb_local CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
GRANT ALL ON wandb_local.* TO 'wandb_local'@'%' WITH GRANT OPTION;
```

### Object Store

The object store can be an externally hosted [Minio cluster](https://docs.min.io/minio/k8s/), or W\&B supports any S3 compatible object store that has support for signed urls. To see if your object store supports signed urls, you can run the [following script](https://gist.github.com/vanpelt/2e018f7313dabf7cca15ad66c2dd9c5b). When connecting to an S3 compatible object store you can specify your credentials in the connection string, i.e.

```yaml
s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME
```

By default we assume 3rd party object stores are not running over HTTPS. If you've configured a trusted SSL certificate for your object store, you can tell us to only connect over tls by adding the `tls` query parameter to the url, i.e.

{% hint style="warning" %}
This will only work if the SSL certificate is trusted. We do not support self-signed certificates.
{% endhint %}

```yaml
s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME?tls=true
```

When using 3rd party object stores, you'll want to set `BUCKET_QUEUE` to `internal://`. This tells the W\&B server to manage all object notifications internally instead of depending on SQS.

The most important things to consider when running your own object store are:

1. **Storage capacity and performance**. It's fine to use magnetic disks, but you should be monitoring the capacity of these disks. Average W\&B usage results in 10's to 100's of Gigabytes. Heavy usage could result in Petabytes of storage consumption.
2. **Fault tolerance.** At a minimum, the physical disk storing the objects should be on a RAID array. Consider running Minio in [distributed mode](https://docs.min.io/minio/baremetal/installation/deploy-minio-distributed.html#deploy-minio-distributed).
3. **Availability.** Monitoring should be configured to ensure the storage is available.

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
            - name: HOST
              value: https://YOUR_DNS_NAME
            - name: LICENSE
              value: XXXXXXXXXXXXXXX
            - name: BUCKET
              value: s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME
            - name: BUCKET_QUEUE
              value: internal://
            - name: AWS_REGION
              value: us-east-1
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
          startupProbe:
            httpGet:
              path: /ready
              port: http
            failureThreshold: 60 # allow 10 minutes for migrations
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
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wandb-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  defaultBackend:
    service:
      name: wandb-service
      port:
        number: 80
```

The k8s YAML above should work in most on-premises installations. However the details of your Ingress and optional SSL termination will vary. See [networking](on-premise-baremetal.md#networking) below.

### Openshift

W\&B supports operating from within an [Openshift kubernetes cluster](https://www.redhat.com/en/technologies/cloud-computing/openshift). Simply follow the instructions in the kubernetes deployment section above.

#### Running the container as an un-privileged user

By default the container will run with a `$UID` of 999.  If you're orchestrator requires the container be run with a non-root user you can specify a `$UID` >= 100000 and a `$GID` of 0.  We must be started as the root group (`$GID=0`) for file system permissions to function properly.  This is the default behavior when running containers in Openshift.  An example security context for kubernetes would looks like:\


```
spec:
  securityContext:
    runAsUser: 100000
    runAsGroup: 0
```

### Docker

You can run _wandb/local_ on any instance that also has Docker installed. We suggest **at least 8GB of RAM and 4vCPU's**. Simply run the following command to launch the container:

```bash
 docker run --rm -d \
   -e HOST=https://YOUR_DNS_NAME \
   -e LICENSE=XXXXX \
   -e BUCKET=s3://$ACCESS_KEY:$SECRET_KEY@$HOST/$BUCKET_NAME \
   -e BUCKET_QUEUE=internal:// \
   -e AWS_REGION=us-east1 \
   -e MYSQL=mysql://$USERNAME:$PASSWORD@$HOSTNAME/$DATABASE \
   -p 8080:8080 --name wandb-local wandb/local
```

{% hint style="warning" %}
You'll want to configure a process manager to ensure this process is restarted if it crashes. A good overview of using SystemD to do this can be [found here](https://blog.container-solutions.com/running-docker-containers-with-systemd).
{% endhint %}

## Networking

### Load Balancer

You'll want to run a load balancer that terminates network requests at the appropriate network boundary. Some customers expose their wandb service on the internet, others only expose it on an internal VPN/VPC. It's important that both the machines being used to execute machine learning payloads and the devices users access the service through web browsers can communicate to this endpoint. Common load balancers include:

1. [Nginx Ingress](https://kubernetes.github.io/ingress-nginx/)
2. [Istio](https://istio.io)
3. [Caddy](https://caddyserver.com)
4. [Cloudflare](https://www.cloudflare.com/load-balancing/)
5. [Apache](https://www.apache.org)
6. [HAProxy](http://www.haproxy.org)

### SSL / TLS

The W\&B server does not terminate SSL. If your security policies require SSL communication within your trusted networks consider using a tool like Istio and [side car containers](https://istio.io/latest/docs/reference/config/networking/sidecar/). The load balancer itself should terminate SSL with a valid certificate. Using self-signed certificates is not supported and will cause a number of challenges for users. If possible using a service like [Let's Encrypt](https://letsencrypt.org) is a great way to provided trusted certificates to your load balancer. Services like Caddy and Cloudflare manage SSL for you.

### Example Nginx Configuration

The following is an example configuration using nginx as a reverse proxy.

```nginx
events {}
http {
    # If we receive X-Forwarded-Proto, pass it through; otherwise, pass along the
    # scheme used to connect to this server
    map $http_x_forwarded_proto $proxy_x_forwarded_proto {
        default $http_x_forwarded_proto;
        ''      $scheme;
    }

    # Also, in the above case, force HTTPS
    map $http_x_forwarded_proto $sts {
        default '';
        "https" "max-age=31536000; includeSubDomains";
    }

    # If we receive X-Forwarded-Host, pass it though; otherwise, pass along $http_host
    map $http_x_forwarded_host $proxy_x_forwarded_host {
        default $http_x_forwarded_host;
        ''      $http_host;
    }

    # If we receive X-Forwarded-Port, pass it through; otherwise, pass along the
    # server port the client connected to
    map $http_x_forwarded_port $proxy_x_forwarded_port {
        default $http_x_forwarded_port;
        ''      $server_port;
    }

    # If we receive Upgrade, set Connection to "upgrade"; otherwise, delete any
    # Connection header that may have been passed to this server
    map $http_upgrade $proxy_connection {
        default upgrade;
        '' close;
    }

    server {
        listen 443 ssl;
        server_name         www.example.com;
        ssl_certificate     www.example.com.crt;
        ssl_certificate_key www.example.com.key;
        
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $proxy_connection;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $proxy_x_forwarded_proto;
        proxy_set_header X-Forwarded-Host $proxy_x_forwarded_host;

        location / {
            proxy_pass  http://$YOUR_UPSTREAM_SERVER_IP:8080/;
        }

        keepalive_timeout 10;
    }
}
```

## Verifying your installation

Regardless of how your server was installed, it's a good idea everything is configured properly. W\&B makes it easy to verify everything is properly configured by using our CLI.

```bash
pip install wandb
wandb login --host=https://YOUR_DNS_DOMAIN
wandb verify
```

If you see any errors contact W\&B support staff. You can also see any errors the application hit at startup by checking the logs.

### Docker

```bash
docker logs wandb-local
```

### Kubernetes

```bash
kubectl get pods
kubectl logs wandb-XXXXX-XXXXX
```
