---
description: Hosting W&B Server on your private cloud.
---

# Private Cloud

A W\&B Server running in a scalable deployment on your private AWS or GCP infrastructure, in your chosen region, and connected to a scalable data store. The environment can be provisioned by us or by your company, using a toolset comprised of Terraform and Kubernetes.

Talk to our sales team by reaching out to [contact@wandb.com](mailto:contact@wandb.com).

## &#x20;Requirements

A private cloud deployment requires the following cloud resources in your account:

* A Kubernetes cluster (EKS or GKE)
* A SQL database (RDS, Google Cloud SQL, or Azure MySQL)
* A object store (S3, GCS, or Azure Blob Store)

{% hint style="info" %}
Check out a [video tutorial](https://www.youtube.com/watch?v=bYmLY5fT2oA) for getting set up using [Terraform](https://www.terraform.io) on AWS!
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=bYmLY5fT2oA" %}

## Amazon Web Services

The simplest way to configure W\&B within AWS is to use our [official Terraform](https://github.com/wandb/terraform-aws-wandb). Detailed instructions can be found in the README. If instead you want to configure services manually you can find [instructions here](configuration.md#amazon-web-services).

## Microsoft Azure

The simplest way to configure W\&B within Azure is to use our [official Terraform](https://github.com/wandb/local/tree/main/terraform/azure). Detailed instructions can be found in the README. If instead you want to configure services manually you can find [instructions here](configuration.md#azure).

## Google Cloud Platform

The simplest way to configure W\&B within GCP is to use our [official Terraform](https://github.com/wandb/terraform-google-wandb). Detailed instructions can be found in the README. If instead you want to configure services manually you can find [instructions here](configuration.md#google-cloud-platform).
