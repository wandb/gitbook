---
description: How to configure the W&B Local Server installation
---

# Advanced Configuration

Your W\&B Local Server comes up ready-to-use on boot. However, several advanced configuration options are available, at the `/system-admin` page on your server once it's up and running. You can email [contact@wandb.com](mailto:contact@wandb.com) to request a trial license to enable more users and teams.

The following are detailed information about manually configuring your local instance. When possible we suggest you use our [existing Terraform](https://github.com/wandb/local) to configure your instance.

## Configuration as code

All configuration settings can be set via the UI however if you would like to manage these configuration options via code you can set the following environment variables:

| Environment Variable | Description                                                                                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LICENSE              | Your wandb/local license                                                                                                                                                                   |
| MYSQL                | The MySQL connection string                                                                                                                                                                |
| BUCKET               | The S3 / GCS bucket for storing data                                                                                                                                                       |
| BUCKET\_QUEUE        | The SQS / Google PubSub queue for object creation events                                                                                                                                   |
| NOTIFICATIONS\_QUEUE | The SQS queue on which to publish run events                                                                                                                                               |
| AWS\_REGION          | The AWS Region where your bucket lives                                                                                                                                                     |
| HOST                 | The FQD of your instance, i.e. [https://my.domain.net](https://my.domain.net)                                                                                                              |
| OIDC\_ISSUER         | A url to your Open ID Connect identity provider, i.e. [https://cognito-idp.us-east-1.amazonaws.com/us-east-1\_uiIFNdacd](https://cognito-idp.us-east-1.amazonaws.com/us-east-1\_uiIFNdacd) |
| OIDC\_CLIENT\_ID     | The Client ID of application in your identity provider                                                                                                                                     |
| OIDC\_AUTH\_METHOD   | implicit (default) or pkce, see below for more context.                                                                                                                                    |
| SLACK\_CLIENT\_ID    | The client ID of the Slack application you want to use for alerts                                                                                                                          |
| SLACK\_SECRET        | The secret of the Slack application you want to use for alerts                                                                                                                             |
| LOCAL\_RESTORE       | You can temporarily set this to true if you're unable to access your instance. Check the logs from the container for temporary credentials.                                                |

## SSO & Authentication

By default, a W\&B Local Server runs with manual user management. Licensed versions of _wandb/local_ also unlock SSO. W\&B can configure an [Auth0](https://auth0.com) tenant for you with any Identity provider they support such as SAML, Ping Federate, Active Directory, etc. Just reach out to your account executive to schedule a setup call with one of our engineers. If you already use Auth0 or have an Open ID Connect compatible server, you can follow the instructions below.

### Open ID Connect

_wandb/local_ uses Open ID Connect for authentication. When creating an application client in your IPD you should choose Web Application or Public Client. For example, if your using AWS Cognito as an identity provider you would choose Public Client:

![Because we're only using OIDC for authentication and not authorization, public clients simplify setup](<../../.gitbook/assets/image (165).png>)

To configure an application client in your identity provider you'll need to provide an allowed callback url:

* Add the following allowed Callback URL `http(s)://YOUR-W&B-HOST/oidc/callback`
* If your IDP supports universal logout, set Logout URL to `http(s)://YOUR-W&B-HOST`

For example, in [AWS Cognito](https://aws.amazon.com/cognito/) if your application was running at `https://wandb.mycompany.com`:

![If your instance is accessible from multiple hosts, be sure to include all of them here.](<../../.gitbook/assets/image (163).png>)

_wandb/local_ will use the ["implicit" grant with the "form\_post" response type](https://auth0.com/docs/get-started/authentication-and-authorization-flow/implicit-flow-with-form-post) by default. You can also configure _wandb/local_ to perform an "authorization\_code" grant using the [PKCE Code Exchange](https://www.oauth.com/oauth2-servers/pkce/) flow. We request the following scopes for the grant: "openid", "profile", and "email". Your identity provider will need to allow these scopes. For example in AWS Cognito the application should look like:

![openid, profile, and email are required](<../../.gitbook/assets/image (168).png>)

To tell _wandb/local_ which grant to use you can select the Auth Method in the settings page or set the OIDC\_AUTH\_METHOD environment variable.

{% hint style="info" %}
For AWS Cognito providers you must set the Auth Method to "pkce"
{% endhint %}

You'll need a Client ID and the url of your OIDC issuer. The OpenID discovery document must be available at `$OIDC_ISSUER/.well-known/openid-configuration` For example when using AWS Cognito you can generate your issuer url by appending your User Pool ID to the Cognito IDP url from the _User Pools > App Integration_ tab:

![The issuer URL would be https://cognito-idp.us-east-1.amazonaws.com/us-east-1\_uiIFNdacd](<../../.gitbook/assets/image (161).png>)

{% hint style="info" %}
Do not use the "Cognito domain" for the IDP url. Cognito provides it's discovery document at `https://cognito-idp.$REGION.amazonaws.com/$USER_POOL_ID`
{% endhint %}

Once you have everything configured you can provide the Issuer, Client ID, and Auth method to _wandb/local_ via `/system-admin` or the environment variables and SSO will be configured.

![](<../../.gitbook/assets/image (177).png>)

{% hint style="info" %}
If you're unable to login to your instance after configuring SSO, you can restart the instance with the `LOCAL_RESTORE=true` environment variable set. This will output a temporary password to the containers logs and disable SSO. Once you've resolved any issues with SSO, you must remove that environment variable to enable SSO again.
{% endhint %}

## File Storage

By default, a W\&B Enterprise Server saves files to a local data disk with a capacity that you set when you provision your instance. To support limitless file storage, you may configure your server to use an external cloud file storage bucket with an S3-compatible API.

{% hint style="info" %}
You should always specify the bucket you're using with the BUCKET environment variable. This removes the need for a persistent volume as all settings can then be persisted to your bucket.
{% endhint %}

### Amazon Web Services

To use an AWS S3 bucket as the file storage backend for W\&B, you'll need to create a bucket, along with an SQS queue configured to receive object creation notifications from that bucket. Your instance will need permissions to read from this queue.

**Create an SQS Queue**

First, create an SQS Standard Queue. Add a permission for all principals for the `SendMessage`, `ReceiveMessage`, `ChangeMessageVisibility`, `DeleteMessage`, and `GetQueueUrl` actions. (If you'd like you can further lock this down using an advanced policy document)

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

**Grant Permissions to Node Running W\&B**

The node on which W\&B Local is running must be configured to permit access to s3 and SQS. Depending on the type of server deployment you've opted for, you may need to add the following policy statements to your node role:

```
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

**Configure W\&B Server**

Finally, navigate to the W\&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/system-admin`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `s3://<bucket-name>`
* **File Storage Region (AWS only)**: `<region>`
* **Notification Subscription**: `sqs://<queue-name>`

![](<../../.gitbook/assets/file-store (2) (1) (1) (1) (1).png>)

Press "Update settings" to apply the new settings.

### Google Cloud Platform

To use a GCP Storage bucket as a file storage backend for W\&B, you'll need to create a bucket, along with a pubsub topic and subscription configured to receive object creation messages from that bucket.

**Create Pubsub Topic and Subscription**

Navigate to Pub/Sub > Topics in the GCP Console, and click "Create topic". Choose a name and create a topic.

Then click "Create subscription" in the subscriptions table at the bottom of the page. Choose a name, and make sure Delivery Type is set to "Pull". Click "Create".

Make sure the service account or account that your instance is running as has access to this subscription.

**Create Storage Bucket**

Navigate to Storage > Browser in the GCP Console, and click "Create bucket". Make sure to choose "Standard" storage class.

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

To create signed file URLs, your W\&B instance also needs the `iam.serviceAccounts.signBlob` permission in GCP. You can add it by adding the `Service Account Token Creator` role to the service account or IAM member that your instance is running as.

**Grant Permissions to Node Running W\&B**

The node on which W\&B Local is running must be configured to permit access to s3 and sqs. Depending on the type of server deployment you've opted for, you may need to add the following policy statements to your node role:

```
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

**Configure W\&B Server**

Finally, navigate to the W\&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/system-admin`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `gs://<bucket-name>`
* **File Storage Region**: blank
* **Notification Subscription**: `pubsub:/<project-name>/<topic-name>/<subscription-name>`

![](<../../.gitbook/assets/file-store (2) (1) (1) (1) (1) (1).png>)

Press "update settings" to apply the new settings.

### Azure

To use an Azure blob container as the file storage for W\&B, you'll need to create a storage account (if you don't already have one you want to use), create a blob container and a queue within that storage account, and then create an event subscription that sends "blob created" notifications to the queue from the blob container.

#### Create a Storage Account

If you have a storage account you want to use already, you can skip this step.

Navigate to [Storage Accounts > Add ](https://portal.azure.com/#create/Microsoft.StorageAccount)in the Azure portal. Select an Azure subscription, and select any resource group or create a new one. Enter a name for your storage account.

![Azure storage account setup](<../../.gitbook/assets/image (42).png>)

Click Review and Create, and then, on the summary screen, click Create:

![Azure storage account details review](<../../.gitbook/assets/image (41).png>)

#### Creating the blob container

Go to [Storage Accounts](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) in the Azure portal, and click on your new storage account. In the storage account dashboard, click on Blob service > Containers in the menu:

![](<../../.gitbook/assets/image (43).png>)

Create a new container, and set it to Private:

![](<../../.gitbook/assets/image (50).png>)

Go to Settings > CORS > Blob service, and enter the IP of your wandb server as an allowed origin, with allowed methods `GET` and `PUT`, and all headers allowed and exposed, then save your CORS settings.

![](<../../.gitbook/assets/image (46).png>)

#### Creating the Queue

Go to Queue service > Queues in your storage account, and create a new Queue:

![](<../../.gitbook/assets/image (51).png>)

Go to Events in your storage account, and create an event subscription:

![](<../../.gitbook/assets/image (47).png>)

Give the event subscription the Event Schema "Event Grid Schema", filter to only the "Blob Created" event type, set the Endpoint Type to Storage Queues, and then select the storage account/queue as the endpoint.

![](<../../.gitbook/assets/image (52).png>)

In the Filters tab, enable subject filtering for subjects beginning with `/blobServices/default/containers/your-blob-container-name/blobs/`

![](<../../.gitbook/assets/image (53).png>)

#### Configure W\&B Server

Go to Settings > Access keys in your storage account, click "Show keys", and then copy either key1 > Key or key2 > Key. Set this key on your W\&B server as the environment variable `AZURE_STORAGE_KEY`.

![](<../../.gitbook/assets/image (54).png>)

Finally, navigate to the W\&B settings page at `http(s)://YOUR-W&B-SERVER-HOST/system-admin`. Enable the "Use an external file storage backend" option, and fill in the s3 bucket, region, and SQS queue in the following format:

* **File Storage Bucket**: `az://<storage-account-name>/<blob-container-name>`
* **Notification Subscription**: `az://<storage-account-name>/<queue-name>`

![](<../../.gitbook/assets/image (55).png>)

Press "Update settings" to apply the new settings.

## Slack

In order to integrate your local W\&B installation with Slack, you'll need to create a suitable Slack application.

#### Creating the Slack application

Visit [https://api.slack.com/apps](https://api.slack.com/apps) and select **Create New App** in the top right.

![](<../../.gitbook/assets/image (56).png>)

You can name it whatever you like, but what's important is to select the same Slack workspace as the one you intend to use for alerts.

![](<../../.gitbook/assets/image (124).png>)

#### Configuring the Slack application

Now that we have a Slack application ready, we need to authorize for use as an OAuth bot. Select **OAuth & Permissions** in the sidebar to the left.

![](<../../.gitbook/assets/image (57).png>)

Under **Scopes**, supply the bot with the **incoming\_webhook** scope.

![](<../../.gitbook/assets/image (128) (1).png>)

Finally, configure the **Redirect URL** to point to your W\&B installation. You should use the same value as what you set **Frontend Host** to in your local system settings. You can specify multiple URLs if you have different DNS mappings to your instance.

![](<../../.gitbook/assets/image (58).png>)

Hit **Save URLs** once finished.

To further secure your Slack application and prevent abuse, you can specify an IP range under **Restrict API Token Usage**, whitelisting the IP or IP range of your W\&B instance(s).

#### Register your Slack application with W\&B

Navigate to the **System Settings** page of your W\&B instance. Check the box to enable a custom Slack application:

![](<../../.gitbook/assets/image (60).png>)

You'll need to supply your Slack application's client ID and secret, which you can find in the **Basic Information** tab.

![](<../../.gitbook/assets/image (61).png>)

That's it! You can now verify that everything is working by setting up a Slack integration in the W\&B app. Visit [this page](../../ref/app/features/alerts.md) for more detailed information.
