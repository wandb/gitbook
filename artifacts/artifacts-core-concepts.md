# Artifacts Core Concepts

In this guide you will learn everything you need to know to hit the ground running with W&B Artifacts. Let's get started!

## The Big Picture <a id="the-big-picture"></a>

W&B Artifacts was designed to make it effortless to version your datasets and models, regardless of whether you want to store your files with us or whether you already have a bucket you want us to track. Once you've tracked your dataset or model files, W&B will automatically log each and every modification, giving you a complete and auditable history of changes to your files. This lets you focus on the fun and important parts of evolving your datasets and training your models, while W&B handles the otherwise tedious process of tracking all the details.

## Terminology <a id="terminology"></a>

Let's start with a few definitions. First off, what exactly do we mean by the word "artifact"?

Conceptually, an **artifact** is simply a directory in which you can store whatever you want, be it images, HTML, code, audio, or raw binary data. You can use it the same way you would an S3 or Google Cloud Storage bucket. Every time you change the contents of this directory, W&B will create a new **version** of your artifact instead of simply overwriting the previous contents.

Assume we have the following directory structure:

```text
images|-- cat.png (2MB)|-- dog.png (1MB)
```

Let's log it as the first version of a new artifact, `animals`:

```text
#!/usr/bin/env python#log_artifact.pyimport wandb​run = wandb.init()artifact = wandb.Artifact('animals', type='dataset')artifact.add_dir('images')run.log_artifact(artifact) # Creates `animals:v0`
```

In W&B parlance, this version has the **index** `v0`. Every new version of an artifact bumps the index by one. You can imagine that once you have hundreds of versions, referring to a specific version by its index would be confusing and error prone. This is where **aliases** come in handy. An alias allows you to apply a human-readable name to given version.

To make this more concrete, let's say we want to update our dataset with a new image and mark the new version as our `latest` image. Here's our new directory structure:

```text
images|-- cat.png (2MB)|-- dog.png (1MB)|-- rat.png (3MB)
```

Now, we can simply rerun `log_artifact.py` to produce `animals:v1`. W&B will automatically assign the newest version the alias `latest`, so instead of using the version index we could also refer to it using `animals:latest`. You can customize the aliases to apply to a version by passing in `aliases=['my-cool-alias']` to `log_artifact`.

Referring to artifacts is easy. In our training script, here's all we need to do to pull in the current the newest version of your dataset:

```text
import wandb​run = wandb.init()animals = run.use_artifact('animals:latest')directory = animals.download()​# Train on our image dataset...
```

That's it! Now, whenever we want to make changes to our dataset we can just run `log_artifact.py` again and W&B will take care of the rest. The training scripts will then automatically pull in the `latest` version.

## Storage Layout <a id="storage-layout"></a>

As an artifact evolves over time and versions start to accumulate, you may start to worry about the space requirements of storing all the iterations of the dataset. Luckily, W&B stores artifact files such that only the files that changed between two versions incur a storage cost.

Let's refer back to `animals:v0` and `animals:v1`, which track the following contents:

```text
images|-- cat.png (2MB) # Added in `v0`|-- dog.png (1MB) # Added in `v0`|-- rat.png (3MB) # Added in `v1`
```

While `v1` tracks a total of 6MB worth of files, it only takes up 3MB of space because it shares the remaining 3MB in common with `v0`. If you were to delete `v1`, you would reclaim the 3MB associated with `rat.png`. On the other hand, if you were to delete `v0` then `v1` would need to inherit the storage costs of `cat.png` and `dog.png` bringing its size to 6MB.

## Data Privacy & Compliance <a id="data-privacy-and-compliance"></a>

When logging artifacts, the files are uploaded to Google Cloud bucket managed by W&B. The contents of the bucket are encrypted both at rest and in transit. Moreover, the artifact files are only visible to users who have access to the corresponding project.![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUtth0QHsFU8uW3xGHU%2F-MUtuFRyuOIF0Kzofe5f%2Fimage.png?alt=media&token=4a4d710c-4ee6-4c92-8b08-1b3c79074cf8)

When you delete a version of an artifact, all the files that can be safely deleted \(meaning the files are not used in previous or subsequent versions\) are _immediately_ removed from our bucket. Similarly, when you delete an entire artifact _all_ of its contents are removed from our bucket.

For sensitive datasets that cannot reside in a multi-tenant environment, you can use either a private W&B server connected to your cloud bucket or **reference artifacts**. Reference artifacts maintain links to files on your buckets or servers, meaning that W&B only keeps track of the metadata associated with the files and not the files themselves.![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MUtth0QHsFU8uW3xGHU%2F-MUtuIUyqweoWl7ACTWA%2Fimage.png?alt=media&token=7a26ccc4-f86c-456e-9603-c77da642fff1)

Building reference artifacts works the same as a regular artifact:

```text
import wandb​run = wandb.init()artifact = wandb.Artifact('animals', type='dataset')artifact.add_reference('s3://my-bucket/animals')
```

You maintain control over the bucket and its files, while W&B just keeps track of all the metadata on your behalf.[  
](https://docs.wandb.ai/artifacts)

