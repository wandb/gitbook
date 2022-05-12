---
description: >-
  Frequently asked questions about setting up privately-hosted versions of our
  app
---

# FAQ

**How can I switch back to the cloud after using local?**\
To restore a machine to reporting metrics to our cloud hosted solution, run `wandb login --cloud`.

**Does my server need a connection to the internet?**\
No internet connection needed. W\&B Local can run in air gapped environments. The only requirement is that the machines that train your models on can connect to the server hosting your W\&B instance, so that data can be sync'd to your private-hosted dashboard.

**Where is my data stored?**\
The default docker image runs MySQL and Minio inside of the container and writes all data in sub folders of `/vol` . You can configure external MySQL and Object Storage by getting a license. Email [contact@wandb.com](mailto:contact@wandb.com) for more details.

**Can I run a wandb server in my own datacenter?**\
Yes, but you are responsible for running your own MySQL 5.7 database and Object Store as [described in Production Setup](setup/#on-premise-baremetal). We strongly recommend running our server within a cloud vendor as the operational expertise and resources needed to operate a scalable MySQL 5.7 database and Object Store is non-trivial.

**How often do you release upgrades?**\
We strive to release upgraded versions of our server at least once a month.

**What happens if my server goes down?**\
Experiments that are in progress will enter a backoff retry loop and continue attempting to connect to your local instance for 24 hours to sync the data.

**What happens if I run out of storage?**\
Make sure you configure **external metadata and object stores** to avoid risking permanent data loss. There are no backups of the database if the disk runs out of space. The instance will stop working.

**What are the scaling characteristics of this service?**\
A single instance of _wandb/local_ without an external MySQL store will scale to up to 10's concurrent experiments being tracked at once. Instances connected to an external MySQL store will scale to 100's of concurrent runs. If you have a need for tracking more concurrent experiments send us a note at [contact@wandb.com](mailto:contact@wandb.com) to inquire about our multi instance high availability installation options.

**How do I do a factory reset if I can't access my instance?**\
If you're unable to connect to your instance you can put it in restore mode by setting the LOCAL\_RESTORE environment variable when you start local. If you're starting wandb local using our cli you can do so with `wandb local -e LOCAL_RESTORE=true` Look at the logs printed on startup for a temporary username / password to access the instance.

**Does a wandb server need read or write access to the S3 bucket?**\
Yes to both. The wandb server needs to be able to read from the bucket in order to generate signed URLs for use by clients, and it needs to have write access in order to update file metadata (see section '[Grant Permissions to Node Running W\&B](https://docs.wandb.ai/guides/self-hosted/configuration#amazon-web-services)'). Because the server generates temporary signed URLs for use by clients, there’s no need to make the s3 bucket public or explicitly grant permissions to any end-users.

**Can I use environment variables to store my token?**\
You can set `WANDB_API_KEY` and `WANDB_BASE_URL` environment variables.

### Admin Functionality

#### How do I find the version of Local I have?

This ability is given to admins by clicking on your profile picture on the top right of the dashboard. From there, navigate to 'System Settings' and you'll see the local instance version you are using.

#### How do I manage the people on my instance?

You are able to take advantage of admin functionality by going to:  `http://<deployed_name>/admin/users` and clicking on the icon with three horizontal lines. This will allow you to invites users to your instance, reset passwords, deactivate, and delete users from your `wandb` instance.

#### Does W\&B support Role Based Access Control (RBAC)?

Yes, W\&B has RBAC controls at a team level where in only members invited to the team can view any activity inside that workspace. This can also be managed programmatically using the `wandb` python SDK.

#### Does self-hosted W\&B allow setting up an external SMTP server?

Yes, W\&B supports setting up an external SMTP server. Please see below for steps to setup:

* Set the `GORILLA_EMAIL_SINK` environment variable in the docker container or the Kubernetes deployment to `smtp://<user:password>@smtp.host.com:<port>`
* `user` and `password` are optional, if you’re using an SMTP server that’s designed to be unauthenticated you would just set the value for the environment variable like `GORILLA_EMAIL_SINK=smtp://smtp.host.com:<port>`
* Common used port number for SMTP is port `25`. Note that this might be different based on your setup.

**How to fix MySQL 5.7 `max_prepared_stmt_count` error?**

Usually the `max_prepared_stmt_count` values range from `0-1048576` with the default being `16382`. If you're running into this error, contact your DB admin to update the `max_prepared_stmt_count` to `1048576` and the error should be resolved.

#### How can we fix the `Error 1040: Too many connections` error?

* This error originates from the MySQL database when there are too many parallel connections but the `max_connections` variable has a lower threshold value.
* To fix this error, ask your instance administrator to login to the mysql instance
* Then type `show variables like "max_connections"`
*   This will return something similar to

    | Variable\_name   | Value |
    | ---------------- | ----- |
    | max\_connections | 100   |
* Based on the type and instance of the database used the max\_connections allowed by the database can range from 100-16400
*   To update the limit, simply issue the command:

    `set global max_connections = 16400;`
* This will update the `max_connections` allowed on the database



