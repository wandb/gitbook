---
description: >-
  The enterprise solution for private cloud or on-prem hosting of Weights &
  Biases
---

# Self-Hosting

Use Local to host the W\&B App on your own servers, either private cloud or on-prem. To quickly test Local, you can even run the app from your laptop. Local gives you the power of W\&B tracking and visualization without sending data to the W\&B cloud servers.

We also offer [W\&B Enterprise Cloud](cloud.md), which runs a completely scalable infrastructure within your company's AWS or GCP account. This system is a good choice for massively scalable experiment tracking.

## Setup Guide

### 1. Install W\&B locally with Docker

On any machine with [Docker](https://www.docker.com) and [Python](https://www.python.org) installed, run

```
pip install wandb
wandb local
```

to pull the latest version of [our Docker image from Docker Hub](https://hub.docker.com/r/wandb/local) and run the container on your system. The full command, in case you want to modify the port or other details, is

```bash
docker run --rm -d -v wandb:/vol -p 8080:8080 --name wandb-local wandb/local
```

This runs a **local** instance of wandb on your machine: all your training logs and metrics will be stored locally inside `/vol` and visible at [http://localhost:8080](http://localhost:8080) on your machine.

Once the container spins up, you will be able to create a new user account and will be prompted to login to wandb by fetching the API key for this new account from [http://localhost:8080/authorize](http://localhost:8080/authorize). To start logging locally, [install the wandb Python package](https://github.com/wandb/client) (e.g. from the command line, `pip install wandb`). Then run any script like the toy example below (or [any of these](https://github.com/wandb/examples)).

```python
import wandb

# 1. Start a new W&B experiment run
wandb.init(project="my_project")

# 2. Save any model configuration
wandb.config.learning_rate = 0.01

# [train your model]

# 3. Log metrics to visualize model performance
for i in range(10):
	wandb.log({"loss" : 10-i, "acc" : float(i)/10.0})
```

### **2. Set up a new user account**

The first time you start a `wandb/local` instance you will be prompted to create an account by visiting http://localhost:8080. This account is only stored locally in your installation and it's what you will use to authenticate with the service.

### 3. Get a free license

[**Open the Deploy Manager** ](https://deploy.wandb.ai/deploy)to get a free license. We offer two options:

1. [**Personal licenses ->**](https://deploy.wandb.ai/deploy) are free forever for personal work:\
   ![](<../../.gitbook/assets/image (174).png>)
2.  [**Team trial licenses ->**](https://deploy.wandb.ai/deploy) are free and last 30 days, allowing you to set up a team and connect a scalable backend:

    ![](<../../.gitbook/assets/image (161) (1).png>)

[**Contact sales -**](https://wandb.ai/site/local-contact)**>** to learn more about Enterprise Self-Hosting options for W\&B.

### 4. Create and scale a shared instance

This private instance of W\&B is excellent for initial testing. To enjoy the powerful collaborative features of W\&B, you will need a shared instance on a central server, which you can [set up on AWS, GCP, Azure, Kubernetes, or Docker](https://docs.wandb.ai/self-hosted/setup).

{% hint style="warning" %}
**Trial Mode vs. Production Setup**

In Trial Mode of W\&B Local, you're running the Docker container on a single machine. This setup is quick and painless, and it's great for testing the product, but it isn't scalable in the long term.

Once you're ready to move from test projects to real production work, it is crucial that you set up a scalable file system to avoid data loss: allocate extra space in advance, resize the file system proactively as you log more data, and configure external metadata and object stores for backup. If you run out of disk space, the instance will stop working, and additional data will be lost.
{% endhint %}

### 5. Set training code to log to local server

If you're running wandb on multiple machines or switching between a local instance and the wandb cloud, there are several ways to control where your runs are logged. If you want to send metrics to the shared **local** instance and you've configured DNS, you can

* set the host flag to the address of the local instance whenever you login:

```
 wandb login --host=http://wandb.your-shared-local-host.com
```

* set the environment variable `WANDB_BASE_URL` to the address of the local instance:

```python
export WANDB_BASE_URL = "http://wandb.your-shared-local-host.com"
```

In an automated environment, you can set the `WANDB_API_KEY` which is accessible at [wandb.your-shared-local-host.com/authorize](http://wandb.your-shared-local-host.com/authorize).

To switch to logging to the public **cloud** instance of wandb, set the host to `api.wandb.ai`:

```
wandb login --cloud
```

or

```python
export WANDB_BASE_URL = "https://api.wandb.ai"
```

You can also switch to your cloud API key, available at [https://wandb.ai/settings](https://wandb.ai/settings) when you're logged in to your cloud-hosted wandb account in your browser.

### 6. Request a team license

Contact us at [support@wandb.com](mailto:support@wandb.com) to request an upgrade to your license. We can give you a free trial of up to 3 users in your local install. We also provide free teams for academics and open source project teams. Reach out to discuss options.
