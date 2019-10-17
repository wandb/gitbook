---
description: How to configure the W&B Enterprise Server installation
---

# Configuration

Your W&B Enterprise Server comes up ready-to-use on boot. However, several advanced configuration options are available, at the `/vm-settings` page on your server once it's up and running.

## Authentication

By default, a W&B Enterprise Server runs in "single-user mode": right from booting up, you can log in and start sending data to your server. However, to unlock the full sharing functionality of W&B, you should configure authentication.

Your server supports any authentication provider supported by [Auth0](https://auth0.com/). You should set up your own Auth0 domain and application that will be under your teams' control.

After creating an Auth0 app, you'll need to configure your Auth0 callbacks to the host of your W&B Server. By default, the server supports http from the public or private IP address provided by the host. You can also configure a DNS hostname and SSL certificate if you choose.

* Set the Callback URL to `http(s)://YOUR-W&B-SERVER-HOST`
* Set the Allowed Web Origin to `http(s)://YOUR-W&B-SERVER-HOST`
* Set the Logout URL to `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Auth0 Settings](../.gitbook/assets/auth0-1.png)

Save the Client ID and domain from your Auth0 app.

![Auth0 Settings](../.gitbook/assets/auth0-2.png)

Then, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/vm-settings`. Enable the "Customize Authentication with Auth0" option, and fill in the Client ID and domain from your Auth0 app.

![Enterprise authentication settings](../.gitbook/assets/enterprise-auth.png)

Finally, press "Update settings and restart W&B".

## File Storage

By default, a W&B Enterprise Server saves files to a local data disk with a capacity that you set when you provision your instance. To support limitless file storage, you may configure your server to use an external cloud file storage bucket with an S3-compatible API.

### Amazon Web Services

To use an AWS S3 bucket as the file storage backend for W&B, you'll need to create a bucket, along with an SQS queue configured to receive object creation notifications from that bucket. Your instance will need permissions to read from this queue.

**Create an SQS Queue**

First, create an SQS Standard Queue. Add a permission for all principals for the `SendMessage` and `ReceiveMessage` actions. \(If you like you can further lock this down using an advancd policy document.\)

![Enterprise file storage settings](../.gitbook/assets/sqs-perms.png)

**Create an S3 Bucket and Bucket Notifications**

Then, create an S3 bucket. Under the bucket properties page in the console, in the "Events" section of "Advanced Settings", click "Add notification", and configure all object creation events to be sent to the SQS Queue you configured earlier.

![Enterprise file storage settings](../.gitbook/assets/s3-notification.png)

Enable CORS access: your CORS configuration should look like the following:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/vm-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `s3://<bucket-name>`
* **File Storage Region**: `<region>`
* **Notification Subscription**: `sqs://<queue-name>`

![AWS file storage settings](../.gitbook/assets/aws-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

### Google Cloud Platform

To use a GCP Storage bucket as a file storage backend for W&B, you'll need to create a bucket, along with a pubsub topic and subscription configured to receive object creation messages from that bucket.

**Create Pubsub Topic and Subscription**

Navigate to Pub/Sub &gt; Topics in the GCP Console, and click "Create topic". Choose a name and create a topic.

Then click "Create subscription" in the subscriptions table at the bottom of the page. Choose a name, and make sure Delivery Type is set to "Pull". Click "Create".

**Create Storage Bucket**

Navigate to Storage &gt; Browser in the GCP Console, and click "Create bucket". Make sure to choose "Standard" storage class.

**Create Pubsub Notification**

Creating a notification stream from the Storage Bucket to the Pubsub Topic can unfortunately only be done in the console. Make sure you have `gsutil` installed, and logged into the correct GCP Project, then run the following:

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[Further reference is available on the Cloud Storage website.](https://cloud.google.com/storage/docs/reporting-changes)

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/vm-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `gs://<bucket-name>`
* **File Storage Region**: blank
* **Notification Subscription**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![AWS file storage settings](../.gitbook/assets/gcloud-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

