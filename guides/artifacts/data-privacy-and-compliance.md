# Data Privacy and Compliance

Files are uploaded to Google Cloud bucket managed by Weights & Biases when you log artifacts. The contents of the bucket are encrypted both at rest and in transit. Artifact files are only visible to users who have access to the corresponding project.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

When you delete a version of an artifact, all the files that can be safely deleted (files not used in previous or subsequent versions) are _immediately_ removed from Weights & Biases buckets. Similarly, when you delete an entire artifact, all of its contents are removed from our bucket.

For sensitive datasets that cannot reside in a multi-tenant environment, you can use either a private W\&B server connected to your cloud bucket or _reference artifacts_. Reference artifacts track references to private buckets without sending file contents to W\&B. Reference artifacts maintain links to files on your buckets or servers. In other words, Weights & Biases only keeps track of the metadata associated with the files and not the files themselves.

<figure><img src="../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

Create a reference artifact similar to how you create a non reference artifact:

```python
import wandb

run = wandb.init()
artifact = wandb.Artifact('animals', type='dataset')
artifact.add_reference('s3://my-bucket/animals')
```

For alternatives, contact us at [contact@wandb.com](mailto:contact@wandb.com) to talk about private cloud and on-premises installations.
