---
description: Frequently asked questions about setting up locally-hosted versions of our app
---

# Local FAQ

## Does my server need a connection to the internet?

Nope, _wandb/local_ can run in air gapped environments. The only requirement is that the machines that train your models on can connect to this server over the network.

## Where is my data stored?

The default docker image runs MySQL and Minio inside of the container and writes all data in sub folders of `/vol` . You can configure external MySQL and Object Storage by getting a license. Email [contact@wandb.com](mailto:contact@wandb.com) for more details.

## How often do you release upgrades?

We strive to release upgraded versions of our server at least once a month.

## What happens if my server goes down?

Experiments that are in progress will enter a backoff retry loop and continue attempting to connect for 24 hours.

## What are the scaling characteristics of this service?

A single instance of _wandb/local_ without an external MySQL store will scale to up to 10's concurrent experiments being tracked at once. Instances connected to an external MySQL store will scale to 100's of concurrent runs. If you have a need for tracking more concurrent experiments send us a note at [contact@wandb.com](mailto:contact@wandb.com) to inquire about our multi instance high availability installation options.

## How do I do a factory reset if I can't access my instance?

If you're unable to connect to your instance you can put it in restore mode by setting the LOCAL\_RESTORE environment variable when you start local. If you're starting wandb local using our cli you can do so with `wandb local -e LOCAL_RESTORE=true` Look at the logs printed on startup for a temporary username / password to access the instance.

## How can I switch back to the cloud after using local?

To restore a machine to reporting metrics to our cloud hosted solution, run `wandb login --host=https://api.wandb.ai`.

