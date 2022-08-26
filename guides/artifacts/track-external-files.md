# Track external files

Use **reference artifacts** to track files saved outside the Weights and Biases system, for example in an Amazon S3 bucket, GCS bucket, HTTP file server, or even an NFS share. Log artifacts outside of a [Weights and Biases Run](https://docs.wandb.ai/ref/python/run) with the Weights and Biases [CLI](https://docs.wandb.ai/ref/cli).

### Log artifacts outside of runs

Weights and Biases creates a run when you log an artifact outside of a run. Each artifact belongs to a run, which in turn belongs to a project; an artifact (version) also belongs to a collection, and has a type.

Use the [`wandb artifact put`](https://docs.wandb.ai/ref/cli/wandb-artifact/wandb-artifact-put) command to upload an artifact to the Weights and Biases server outside of a Weights and Biases run. Provide the name of the project you want the artifact to belong to along with the name of the artifact (`project/artifact_name`).Optionally provide the type (`TYPE`). Replace `PATH` in the code snippet below with the file path of the artifact you want to upload.

```bash
$ wandb artifact put --name project/artifact_name --type TYPE PATH
```

Weights and Biases will create a new project if a the project you specify does not exist. For information on how to download an artifact, see [Use an artifact](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/use-an-artifact).

## Track artifacts outside of Weights and Biases

Use Weights and Baises Artifacts for dataset versioning and model lineage, and use **reference artifacts** to track files saved outside the Weights and Biases server. In this mode an artifact only stores metadata about the files, such as URLs, size, and checksums. The underlying data never leaves your system.  See the [Quick Start](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/quick-start) for information on how to save files and directories to Weights and Biases servers instead.

For an example of tracking reference files in GCP, see the [Guide to Tracking Artifacts by Reference](https://wandb.ai/stacey/artifacts/reports/Tracking-Artifacts-by-Reference--Vmlldzo1NDMwOTE).

The following describes how to construct reference artifacts and how to best incorporate them into your workflows.&#x20;

### Amazon S3 / GCS References

Use Weights and Biases Artifacts for dataset and model versioning to track references in cloud storage buckets. With artifact references, seamlessly layer tracking on top of your buckets with no modifications to your existing storage layout.

Artifacts abstract away the underlying cloud storage vendor (such AWS or GCP). Information described the proceeding section apply uniformly both Google Cloud Storage and Amazon S3.

{% hint style="info" %}
Weights and Biases Artifacts support any Amazon S3 compatible interface — including MinIO! The scripts below work, as is, when you set the AWS\_S3\_ENDPOINT\_URL environment variable to point at your MinIO server.
{% endhint %}

Assume we have a bucket with the following structure:

```
s3://my-bucket
+-- datasets/
|		+-- mnist/
+-- models/
		+-- cnn/
```

Under `mnist/` we have our dataset, a collection of images. Lets track it with an artifact:

```python
import wandb

run = wandb.init()
artifact = wandb.Artifact('mnist', type='dataset')
artifact.add_reference('s3://my-bucket/datasets/mnist')
run.log_artifact(artifact)
```

{% hint style="warning" %}
By default, W\&B imposes a 10,000 object limit when adding an object prefix. You can adjust this limit by specifying `max_objects=` in calls to `add_reference`.
{% endhint %}

Our new reference artifact `mnist:latest` looks and behaves similarly to a regular artifact. The only difference is that the artifact only consists of metadata about the S3/GCS object such as its ETag, size, and version ID (if object versioning is enabled on the bucket).

Weights and Biases will attempt to use the corresponding credential files or environment variables associated with the cloud provider when it adds references to Amazon S3 or GCS buckets.

| Priority                    | Amazon S3                                                                                                           | Google Cloud Storage                                          |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| 1 - Environment variables   | <p><code>AWS_ACCESS_KEY_ID</code></p><p><code>AWS_SECRET_ACCESS_KEY</code></p><p><code>AWS_SESSION_TOKEN</code></p> | `GOOGLE_APPLICATION_CREDENTIALS`                              |
| 2 - Shared credentials file | `~/.aws/credentials`                                                                                                | `application_default_credentials.json` in `~/.config/gcloud/` |
| 3 - Config file             | `~/.aws.config`                                                                                                     | N/A                                                           |

Interact with this artifact similarly to a normal artifact. In the App UI, you can look through the contents of the reference artifact using the file browser, explore the full dependency graph, and scan through the versioned history of your artifact.

{% hint style="warning" %}
Rich media such as images, audio, video, and point clouds may fail to render in the App UI depending on the CORS configuration of your bucket. Allow listing **app.wandb.ai** in your bucket's CORS settings will allow the App UI to properly render such rich media.

Panels might fail to render in the App UI for private buckets. If your company has a VPN, you could update your bucket's access policy to whitelist IPs within your VPN.
{% endhint %}

### Download a reference artifact

```python
import wandb

run = wandb.init()
artifact = run.use_artifact('mnist:latest', type='dataset')
artifact_dir = artifact.download()
```

Weights and Biases will use the metadata recorded when the artifact was logged to retrieve the files from the underlying bucket when it downloads a reference artifact. If your bucket has object versioning enabled, Weights and Biases will retrieve the object version corresponding to the state of the file at the time an artifact was logged. This means that as you evolve the contents of your bucket, you can still point to the exact iteration of your data a given model was trained on since the artifact serves as a snapshot of your bucket at the time of training.

{% hint style="info" %}
W\&B recommends that you enable 'Object Versioning' on your Amazon S3 or GCS buckets if you overwrite files as part of your workflow. With versioning enabled on your buckets, artifacts with references to files that have been overwritten will still be intact because the older object versions are retained.
{% endhint %}

### Tying it together

The following code example demonstrates a simple workflow you can use to track a dataset in Amazon S3 or GCS that feeds into a training job:

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

For an example of tracking reference files in GCP, with code and screenshots, follow our [Guide to Tracking Artifacts by Reference](https://wandb.ai/stacey/artifacts/reports/Tracking-Artifacts-by-Reference--Vmlldzo1NDMwOTE).

### Filesystem References

Another common pattern for fast access to datasets is to expose an NFS mount point to a remote filesystem on all machines running training jobs. This can be an even simpler solution than a cloud storage bucket because from the perspective of the training script, the files look just like they are sitting on your local filesystem. Luckily, that ease of use extends into using Artifacts to track references to filesystems — mounted or otherwise.

Assume we have a filesystem mounted at `/mount` with the following structure:

```
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

By default, W\&B imposes a 10,000 file limit when adding a reference to a directory. You can adjust this limit by specifying `max_objects=` in calls to `add_reference`.

Note the triple slash in the URL. The first component is the `file://` prefix that denotes the use of filesystem references. The second is the path to our dataset, `/mount/datasets/mnist/`.

The resulting artifact `mnist:latest` looks and acts just like a regular artifact. The only difference is that the artifact only consists of metadata about the files, such as their sizes and MD5 checksums. The files themselves never leave your system.

You can interact with this artifact just as you would a normal artifact. In the UI, you can browse the contents of the reference artifact using the file browser, explore the full dependency graph, and scan through the versioned history of your artifact. However, the UI will not be able to render rich media such as images, audio, etc. as the data itself is not contained within the artifact.

Downloading a reference artifact is simple:

```python
import wandb

run = wandb.init()
artifact = run.use_artifact('entity/project/mnist:latest', type='dataset')
artifact_dir = artifact.download()
```

For filesystem references, a `download()` operation copies the files from the referenced paths to construct the artifact directory. In the above example, the contents of `/mount/datasets/mnist` will be copied into the directory `artifacts/mnist:v0/`. If an artifact contains a reference to a file that was overwritten, then `download()` will throw an error as the artifact can no longer be reconstructed.

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
