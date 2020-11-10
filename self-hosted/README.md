---
description: Self Hosted installations for projects with sensitive data
---

# Self Hosted

W&B Local is the self hosted version of [Weights & Biases](https://app.wandb.ai). It makes collaborative experiment tracking possible for enterprise machine learning teams, giving you a way to keep all training data and metadata within your organization's network.

[Request a demo to try out W&B Local →](https://www.wandb.com/demo)

We also offer [W&B Enterprise Cloud](cloud.md), which runs a completely scalable infrastructure within your company's AWS or GCP account. This system can scale to any level of usage.

## Features

* Unlimited runs, experiments, and reports
* Keep your data safe on your own company's network
* Integrate with your company's authentication system
* Premier support by the W&B engineering team

The self hosted server is a single Docker image that is simple to deploy. Your W&B data is saved on a persistent volume or an external database so data can be preserved across container versions.

## Server Requirements

The W&B self hosted server requires an instance with at least 4 cores and 8GB memory.

## Self Hosted Resources

{% page-ref page="local.md" %}

{% page-ref page="setup.md" %}

{% page-ref page="configuration.md" %}

{% page-ref page="local-common-questions.md" %}

{% page-ref page="cloud.md" %}

