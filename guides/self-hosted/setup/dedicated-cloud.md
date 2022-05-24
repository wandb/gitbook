---
description: Hosting W&B in a secure enterprise environment.
---

# Dedicated Cloud

In your choice of cloud region, a W\&B Docker container is hosted in a dedicated virtual private network on W\&B's single-tenant AWS or GCP infrastructure. Use our Secure Storage Connector to connect your data to a scalable data store hosted on your company's private cloud.&#x20;

Talk to our sales team by reaching out to [contact@wandb.com](mailto:contact@wandb.com).

## Requirements

A W\&B Dedicated Cloud cloud deployment requires the following cloud resources in your private cloud account:

* An object store (S3 or GCS)
* A KMS key (also called Cloud KMS)

## Setup

To set up the environment, you must run a simple terraform and provide W\&B with the generated output to complete the configuration. The links to the terraform scripts and instructions to run them can be found below for each supported cloud provider.

## Amazon Web Services

The simplest way to configure an object store and a KMS key within AWS is to use our [official Terraform](https://github.com/wandb/terraform-aws-wandb/tree/main/examples/byob). Detailed instructions can be found in the README.

## Google Cloud Platform

The simplest way to configure object store and a KMS key within GCP is to use our [official Terraform](https://github.com/wandb/terraform-google-wandb/tree/main/examples/byob). Detailed instructions can be found in the README.
