---
description: 'Track artifacts saved outside of W&B, for example in a GCP or S3 bucket'
---

# Artifact References

Use Artifacts for dataset versioning and model lineage, and use **reference artifacts** to track files saved outside the W&B system, for example in an S3 bucket, GCS bucket, HTTP file server, or even an NFS share. In this mode an artifact only stores metadata about the files, such as their URLs, sizes, and checksums, while the underlying data never leaves your system. If you'd prefer to save files and directories to W&B servers instead, see the [Walkthrough](api.md).

In this guide, we will explore how to construct reference artifacts and how to best incorporate them into your workflows. Let's dive into it!

## S3 / GCS References

Many organizations using Artifacts for dataset and model versioning are tracking references in cloud storage buckets. With artifact references, seamlessly layer tracking on top of your buckets with no modifications to your existing storage layout.

Artifacts abstracts away the underlying cloud storage vendor \(be it AWS or GCP\), so everything in this section applies uniformly to both Google Cloud Storage and Amazon S3.

{% hint style="info" %}
W&B Artifacts can support any S3 compatible interface — including MinIO! All the scripts below will work as is once you set the `AWS_S3_ENDPOINT_URL` environment variable to point at your MinIO server.
{% endhint %}

Assume we have a bucket with the following structure:

```text
s3://my-bucket
+-- datasets/
|		+-- mnist/
+-- models/
		+-- cnn/
```

Under `mnist/` we have our dataset, a collection of images. Let's track it with an artifact:

```python
import wandb

run = wandb.init()
artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_reference('s3://my-bucket/datasets/mnist')
run.log_artifact(artifact)
```

{% hint style="warning" %}
By default, W&B imposes a 10,000 object limit when adding an object prefix. You can adjust this limit by specifying `max_objects=` in calls to `add_reference`.
{% endhint %}

Our new reference artifact `mnist:latest` looks and acts just like a regular artifact. The only difference is that the artifact only consists of metadata about the S3/GCS object such as its ETag, size, and version ID \(if object versioning is enabled on the bucket\).

When adding references to S3 or GCS buckets, W&B will attempt to use the corresponding credential files or environment variables \(preferring the latter\) associated with the cloud provider, as outlined by the table below:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Priority</th>
      <th style="text-align:left">Amazon S3</th>
      <th style="text-align:left">Google Cloud Storage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1 - Environment variables</td>
      <td style="text-align:left">
        <p>AWS_ACCESS_KEY_ID</p>
        <p>AWS_SECRET_ACCESS_KEY</p>
        <p>AWS_SESSION_TOKEN</p>
      </td>
      <td style="text-align:left">GOOGLE_APPLICATION_CREDENTIALS</td>
    </tr>
    <tr>
      <td style="text-align:left">2 - Shared credentials file</td>
      <td style="text-align:left">~/.aws/credentials</td>
      <td style="text-align:left">application_defualt_credentials.json in ~/.config/gcloud/</td>
    </tr>
    <tr>
      <td style="text-align:left">3 - Config file</td>
      <td style="text-align:left">~/.aws.config</td>
      <td style="text-align:left">N/A</td>
    </tr>
  </tbody>
</table>

You can interact with this artifact just as you would a normal artifact. In the UI, you can browse the contents of the reference artifact using the file browser, explore the full dependency graph, and scan through the versioned history of your artifact.

{% hint style="warning" %}
Rich media such as images, audio, video, and point clouds may fail to render in the UI depending on the CORS configuration of your bucket. Whitelisting **app.wandb.ai** in your bucket's CORS settings will allow the UI to properly render such rich media.

Panels might also fail to render in the UI for private buckets. If your company has a VPN, you could update your bucket's access policy to whitelist IPs within your VPN.
{% endhint %}

Downloading a reference artifact is simple:

```python
import wandb

run = wandb.init()
artifact = run.use_artifact('mnist:latest', type='dataset')
artifact_dir = artifact.download()
```

When downloading a reference artifact, W&B will use the metadata recorded when the artifact was logged to retrieve the files from the underlying bucket. If your bucket has object versioning enabled, then W&B will retrieve the object version corresponding to the state of the file at the time an artifact was logged. This means that as you evolve the contents of your bucket, you can still pinpoint exactly which iteration of your data a given model was trained on since the artifact serves as a snapshot of your bucket at the time of training.

{% hint style="info" %}
W&B recommends that you enable 'Object Versioning' on your S3 or GCS buckets if you overwrite files as part of your workflow. With versioning enabled on your buckets, artifacts with references to files that have been overwritten will still be intact because the older object versions are retained.
{% endhint %}

Putting everything together, here's a simple workflow you can use to track a dataset in S3 or GCS that feeds into a training job:

```python
 import wandb

run = wandb.init()

artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_reference('s3://my-bucket/datasets/mnist')

# Track the artifact and mark it as an input to
# this run in one swoop. A new artifact version
# is only logged if the files in the bucket changed.
run.use_artifact(artifact)

artifact_dir = artifact.download()

# Perform training here...
```

To track models, we can log the model artifact after the training script uploads the model files to the bucket:

```python
import boto3
import wandb

run = wandb.init()

# Training here... 

s3_client = boto3.client('s3')
s3_client.upload_file('my_model.h5', 'my-bucket', 'models/cnn/my_model.h5')

model_artifact = wandb.Artifact('cnn', type='model')
model_artifact.add_reference('s3://my-bucket/models/cnn/')
run.log_artifact(model_artifact)
```

For an in-depth walkthrough of constructing reference artifacts and exploring them in the UI, please check out [https://docs.wandb.ai/artifacts/artifacts-by-reference](https://docs.wandb.ai/artifacts/artifacts-by-reference).

## Filesystem References

Another common pattern for fast access to datasets is to expose an NFS mount point to a remote filesystem on all machines running training jobs. This can be an even simpler solution than a cloud storage buckets because from the perspective of the training script, the files look just like files sitting on your local filesystem. Luckily, that ease of use extends into using W&B Artifacts to track references to filesystems — mounted or otherwise.

Assume we have a filesystem mounted at `/mount` with the following structure:

```text
mount
+-- datasets/
|		+-- mnist/
+-- models/
		+-- cnn/
```

Under `mnist/` we have our dataset, a collection of images. Let's track it with an artifact:

```python
import wandb

run = wandb.init()
artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_reference('file:///mount/datasets/mnist/')
run.log_artifact(artifact)
```

By default, W&B imposes a 10,000 file limit when adding a reference to a directory. You can adjust this limit by specifying `max_objects=` in calls to `add_reference`.

Note the triple slash in the URL. The first component is the `file://` prefix that denotes the use of filesystem references. The second is the path to our dataset, `/mount/datasets/mnist/`.

The resulting artifact `mnist:latest` looks and acts just like a regular artifact. The only difference is that the artifact only consists of metadata about the files, such as their sizes and MD5 checksums. The files themselves never leave your system.

You can interact with this artifact just as you would a normal artifact. In the UI, you can browse the contents of the reference artifact using the file browser, explore the full dependency graph, and scan through the versioned history of your artifact. However, the UI will not be able to render rich media such as images, audio, etc. as the data itself is not contained within the artifact.

Downloading a reference artifact is simple:

```python
import wandb

run = wandb.init()
artifact = run.use_artifact('mnist:latest', type='dataset')
artifact_dir = artifact.download()
```

For filesystem references, a `download()` operation copies the files from the referenced paths to construct the artifact directory. In the above example, the contents of `/mount/datasets/mnist` will be copied into the directory `artifacts/mnist:v0/`. If an artifact contains a reference to a fail that was overwritten, then `download()` will throw an error as the artifact can no longer be reconstructed.

Putting everything together, here's a simple workflow you can use to track a dataset under a mounted filesystem that feeds into a training job:

```python
import wandb

run = wandb.init()

artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_reference('file:///mount/datasets/mnist/')

# Track the artifact and mark it as an input to
# this run in one swoop. A new artifact version
# is only logged if the files under the directory
# changed.
run.use_artifact(artifact)

artifact_dir = artifact.download()

# Perform training here...
```

To track models, we can log the model artifact after the training script writes the model files to the mount point:

```python
import wandb

run = wandb.init()

# Training here...

with open('/mount/cnn/my_model.h5') as f:
	# Output our model file.

model_artifact = wandb.Artifact('cnn', type='model')
model_artifact.add_reference('file:///mount/cnn/my_model.h5')
run.log_artifact(model_artifact)
```



