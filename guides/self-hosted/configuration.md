---
description: How to configure the W&B Local Server installation
---

# Advanced Configuration

Your W&B Local Server comes up ready-to-use on boot. However, several advanced configuration options are available, at the `/system-admin` page on your server once it's up and running. You can email [contact@wandb.com](mailto:contact@wandb.com) to request a trial license to enable more users and teams.

## Configuration as code

All configuration settings can be set via the UI however if you would like to manage these configuration options via code you can set the following environment variables:

| Environment Variable | Description |
| :--- | :--- |
| LICENSE | Your wandb/local license |
| MYSQL | The MySQL connection string |
| BUCKET | The S3 / GCS bucket for storing data |
| BUCKET\_QUEUE | The SQS / Google PubSub queue for object creation events |
| NOTIFICATIONS\_QUEUE | The SQS queue on which to publish run events |
| AWS\_REGION | The AWS Region where your bucket lives |
| HOST | The FQD of your instance, i.e. [https://my.domain.net](https://my.domain.net) |
| AUTH0\_DOMAIN | The Auth0 domain of your tenant |
| AUTH0\_CLIENT\_ID | The Auth0 Client ID of application |
| SLACK\_CLIENT\_ID | The client ID of the Slack application you want to use for alerts |
| SLACK\_SECRET | The secret of the Slack application you want to use for alerts |

## Authentication

By default, a W&B Local Server run with manual user management enabling up to 4 users. Licensed versions of _wandb/local_ also unlock SSO using Auth0.

Your server supports any authentication provider supported by [Auth0](https://auth0.com/). You should set up your own Auth0 domain and application that will be under your teams' control.

After creating an Auth0 app, you'll need to configure your Auth0 callbacks to the host of your W&B Server. By default, the server supports http from the public or private IP address provided by the host. You can also configure a DNS hostname and SSL certificate if you choose.

* Set the Callback URL to `http(s)://YOUR-W&B-SERVER-HOST`
* Set the Allowed Web Origin to `http(s)://YOUR-W&B-SERVER-HOST`
* Set the Logout URL to `http(s)://YOUR-W&B-SERVER-HOST/logout`

![Auth0 Settings](../../.gitbook/assets/auth0-1.png)

Save the Client ID and domain from your Auth0 app.

![Auth0 Settings](../../.gitbook/assets/auth0-2.png)

Then, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Customize Authentication with Auth0" option, and fill in the Client ID and domain from your Auth0 app.

![Enterprise authentication settings](../../.gitbook/assets/enterprise-auth.png)

Finally, press "Update settings and restart W&B".

## File Storage

By default, a W&B Enterprise Server saves files to a local data disk with a capacity that you set when you provision your instance. To support limitless file storage, you may configure your server to use an external cloud file storage bucket with an S3-compatible API.

### Amazon Web Services

To use an AWS S3 bucket as the file storage backend for W&B, you'll need to create a bucket, along with an SQS queue configured to receive object creation notifications from that bucket. Your instance will need permissions to read from this queue.

**Create an SQS Queue**

First, create an SQS Standard Queue. Add a permission for all principals for the `SendMessage`, `ReceiveMessage`, `ChangeMessageVisibility`, `DeleteMessage`, and `GetQueueUrl` actions. \(If you'd like you can further lock this down using an advanced policy document\)

**Create an S3 Bucket and Bucket Notifications**

Then, create an S3 bucket. Under the bucket properties page in the console, in the "Events" section of "Advanced Settings", click "Add notification", and configure all object creation events to be sent to the SQS Queue you configured earlier.

![Enterprise file storage settings](../../.gitbook/assets/s3-notification.png)

Enable CORS access: your CORS configuration should look like the following:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>http://YOUR-W&B-SERVER-IP</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

**Grant Permissions to Node Running W&B**

The node on which W&B Local is running must be configured to permit access to s3 and SQS. Depending on the type of server deployment you've opted for, you may need to add the following policy statements to your node role:

```text
{
   "Statement":[
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":"s3:*",
         "Resource":"arn:aws:s3:::<WANDB_BUCKET>"
      },
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":[
            "sqs:*"
         ],
         "Resource":"arn:aws:sqs:<REGION>:<ACCOUNT>:<WANDB_QUEUE>"
      }
   ]
}
```

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `s3://<bucket-name>`
* **File Storage Region**: `<region>`
* **Notification Subscription**: `sqs://<queue-name>`

![AWS file storage settings](../../.gitbook/assets/aws-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

### Google Cloud Platform

To use a GCP Storage bucket as a file storage backend for W&B, you'll need to create a bucket, along with a pubsub topic and subscription configured to receive object creation messages from that bucket.

**Create Pubsub Topic and Subscription**

Navigate to Pub/Sub &gt; Topics in the GCP Console, and click "Create topic". Choose a name and create a topic.

Then click "Create subscription" in the subscriptions table at the bottom of the page. Choose a name, and make sure Delivery Type is set to "Pull". Click "Create".

Make sure the service account or account that your instance is running as has access to this subscription.

**Create Storage Bucket**

Navigate to Storage &gt; Browser in the GCP Console, and click "Create bucket". Make sure to choose "Standard" storage class.

Make sure the service account or account that your instance is running as has access to this bucket.

**Create Pubsub Notification**

Creating a notification stream from the Storage Bucket to the Pubsub Topic can unfortunately only be done in the console. Make sure you have `gsutil` installed, and logged into the correct GCP Project, then run the following:

```bash
gcloud pubsub topics list  # list names of topics for reference
gsutil ls                  # list names of buckets for reference

# create bucket notification
gsutil notification create -t <TOPIC-NAME> -f json gs://<BUCKET-NAME>
```

[Further reference is available on the Cloud Storage website.](https://cloud.google.com/storage/docs/reporting-changes)

**Add Signing Permissions**

To create signed file URLs, your W&B instance also needs the `iam.serviceAccounts.signBlob` permission in GCP. You can add it by adding the `Service Account Token Creator` role to the service account or IAM member that your instance is running as.

**Grant Permissions to Node Running W&B**

The node on which W&B Local is running must be configured to permit access to s3 and sqs. Depending on the type of server deployment you've opted for, you may need to add the following policy statements to your node role:

```text
{
   "Statement":[
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":"s3:*",
         "Resource":"arn:aws:s3:::<WANDB_BUCKET>"
      },
      {
         "Sid":"",
         "Effect":"Allow",
         "Action":[
            "sqs:*"
         ],
         "Resource":"arn:aws:sqs:<REGION>:<ACCOUNT>:<WANDB_QUEUE>"
      }
   ]
}
```

**Configure W&B Server**

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `gs://<bucket-name>`
* **File Storage Region**: blank
* **Notification Subscription**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![GCP file storage settings](../../.gitbook/assets/gcloud-filestore.png)

Press "update settings and restart W&B" to apply the new settings.

### Azure

To use an Azure blob container as the file storage for W&B, you'll need to create a storage account \(if you don't already have one you want to use\), create a blob container and a queue within that storage account, and then create an event subscription that sends "blob created" notifications to the queue from the blob container.

#### Create a Storage Account

If you have a storage account you want to use already, you can skip this step.

Navigate to [Storage Accounts &gt; Add ](https://portal.azure.com/#create/Microsoft.StorageAccount)in the Azure portal. Select an Azure subscription, and select any resource group or create a new one. Enter a name for your storage account.

![Azure storage account setup](../../.gitbook/assets/image%20%28106%29.png)

Click Review and Create, and then, on the summary screen, click Create:

![Azure storage account details review](../../.gitbook/assets/image%20%28114%29.png)

#### Creating the blob container

Go to  [Storage Accounts](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) in the Azure portal, and click on your new storage account. In the storage account dashboard, click on Blob service &gt; Containers in the menu:

![](../../.gitbook/assets/image%20%28102%29.png)

Create a new container, and set it to Private:

![](../../.gitbook/assets/image%20%28110%29.png)

Go to Settings &gt; CORS &gt; Blob service, and enter the IP of your wandb server as an allowed origin, with allowed methods `GET` and `PUT`, and all headers allowed and exposed, then save your CORS settings.

![](../../.gitbook/assets/image%20%28119%29.png)

#### Creating the Queue

Go to Queue service &gt; Queues in your storage account, and create a new Queue:

![](../../.gitbook/assets/image%20%28101%29.png)

Go to Events in your storage account, and create an event subscription:

![](../../.gitbook/assets/image%20%28108%29.png)

Give the event subscription the Event Schema "Event Grid Schema", filter to only the "Blob Created" event type, set the Endpoint Type to Storage Queues, and then select the storage account/queue as the endpoint.

![](../../.gitbook/assets/image%20%28116%29.png)

In the Filters tab, enable subject filtering for subjects beginning with `/blobServices/default/containers/your-blob-container-name/blobs/`

![](../../.gitbook/assets/image%20%28105%29.png)

#### Configure W&B Server

Go to Settings &gt; Access keys in your storage account, click "Show keys", and then copy either key1 &gt; Key or key2 &gt; Key. Set this key on your W&B server as the environment variable `AZURE_STORAGE_KEY`.

![](../../.gitbook/assets/image%20%28115%29.png)

Finally, navigate to the W&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/admin-settings`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `az://<storage-account-name>/<blob-container-name>`
* **Notification Subscription**: `az://<storage-account-name>/<queue-name>`

![](../../.gitbook/assets/image%20%28109%29.png)

Press "Update settings" to apply the new settings.

## Slack

In order to integrate your local W&B installation with Slack, you'll need to create a suitable Slack application.

#### Creating the Slack application

Visit [https://api.slack.com/apps](https://api.slack.com/apps) and select **Create New App** in the top right.

![](../../.gitbook/assets/image%20%28123%29.png)

You can name it whatever you like, but what's important is to select the same Slack workspace as the one you intend to use for alerts.

![](../../.gitbook/assets/image%20%28124%29.png)

#### Configuring the Slack application

Now that we have a Slack application ready, we need to authorize for use as an OAuth bot. Select **OAuth & Permissions** in the sidebar to the left.

![](../../.gitbook/assets/image%20%28125%29.png)

Under **Scopes**, supply the bot with the **incoming\_webhook** scope.

![](../../.gitbook/assets/image%20%28128%29%20%281%29%20%281%29.png)

Finally, configure the **Redirect URL** to point to your W&B installation. You should use the same value as what you set **Frontend Host** to ****in your local system settings. You can specify multiple URLs if you have different DNS mappings to your instance.

![](../../.gitbook/assets/image%20%28127%29.png)

Hit **Save URLs** once finished.

To further secure your Slack application and prevent abuse, you can specify an IP range under **Restrict API Token Usage**, whitelisting the IP or IP range of your W&B instance\(s\).

#### Register your Slack application with W&B

Navigate to the **System Settings** page of your W&B instance. Check the box to enable a custom Slack application:

![](../../.gitbook/assets/image%20%28126%29.png)

You'll need to supply your Slack application's client ID and secret, which you can find in the **Basic Information** tab.

![](../../.gitbook/assets/image%20%28120%29.png)

That's it! You can now verify that everything is working by setting up a Slack integration in the W&B app. Visit [this page](../../ref/app/features/alerts.md) for more detailed information.

