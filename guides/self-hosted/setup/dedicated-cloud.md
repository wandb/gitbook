---
description: Hosting W&B Server in a secure enterprise environment.
---

# Dedicated Cloud

W\&B is hosted in a dedicated virtual private network on W\&B's single-tenant AWS or GCP infrastructure in your choice of cloud region. Use our Secure Storage Connector to connect your data to a scalable data store hosted on your company's private cloud.

Talk to our sales team by reaching out to [contact@wandb.com](mailto:contact@wandb.com).

## Configuration

We can configure an object store on your behalf, in which case there is no additional configuration that you need to perform. However, we also enable you to connect your object store with our Secure Storage Connector feature. The Secure Storage Connector enables you to connect your own secure data storage to this dedicated cloud infrastructure. This enables you full control of your data with isolation guarantees, while relying on W\&B's cloud operational expertise to reduce your support burden and manage your dedicated infrastructure.&#x20;

A W\&B Dedicated Cloud with Secure Storage Connector enabled requires the following resources in your private cloud account:

* An object store (S3 or GCS)&#x20;
* A KMS key (also called Cloud KMS)&#x20;

__![](<../../../.gitbook/assets/image (167).png>)__

To set up the environment, you must run a simple terraform and provide W\&B with the generated output so that W\&B can complete the configuration. The links to the terraform scripts and instructions to run them can be found below for each supported cloud provider.

## Amazon Web Services

The simplest way to configure an AWS S3 bucket and a KMS key within AWS is to use our [official Terraform](https://github.com/wandb/terraform-aws-wandb/tree/main/examples/byob). Detailed instructions can be found in the README.

## Google Cloud Platform

The simplest way to configure a GCS bucket within GCP is to use our [official Terraform](https://github.com/wandb/terraform-google-wandb/tree/main/examples/byob). Detailed instructions can be found in the README.

## Microsoft Azure Cloud

The simplest way to configure an Azure Blob Storage bucket within Azure is to use our [official Terraform](https://github.com/wandb/terraform-azurerm-wandb/tree/main/examples/byob). Detailed instructions can be found in the README.
