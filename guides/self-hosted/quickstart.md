# Quickstart

This is the simple way to set up W\&B as a local server in a single docker container. For real production workloads, we recommend connecting W\&B to a scalable data store - see [Production Setup](setup.md).

### 1. Install W\&B locally with Docker

On any machine with [Docker](https://www.docker.com) and [Python](https://www.python.org) installed, run

```
pip install wandb
wandb local
```

### **2. Set up a new user account**

The first time you start a `wandb/local` instance you will be prompted to create an account by visiting http://localhost:8080. This account is only stored locally in your installation and it's what you will use to authenticate with the service.

### 3. Get a free license

[**Open the Deploy Manager** ](https://deploy.wandb.ai/deploy)to get a free license.

### 4. Modify training code to log to wandb local server

On your machine where the training code runs, set the host flag to the address of the local instance whenever you login:

```
 wandb login --host=http://wandb.your-shared-local-host.com
```

This private instance of W\&B is excellent for initial testing. To enjoy the powerful collaborative features of W\&B, you will need a shared instance on a central server, which you can [set up on AWS, GCP, Azure, Kubernetes, or Docker](https://docs.wandb.ai/self-hosted/setup).
