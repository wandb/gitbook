# Image Logging De-Duplication

{% hint style="info" %}
Executable companion code can be found here: [https://colab.research.google.com/drive/15LVKk02VCZfEKRQWTg019JMSIbrqPAah?usp=sharing](https://colab.research.google.com/drive/15LVKk02VCZfEKRQWTg019JMSIbrqPAah?usp=sharing)
{% endhint %}

In this guide, you will learn how to upload an image to W\&B once, even while logging the image across multiple runs!

### Step 1: Add your Images to an Artifact

```python
wandb.init()
art = wandb.Artifact("my_images", "dataset")
for path in IMAGE_PATHS:
    art.add(wandb.Image(path), path)
art.log_artifact(art)
```

### Step 2: Use Artifact Images for Logging

The `img_1` object is a `wandb.Image` which retains a reference to its source artifact. Logging it to a run (or another artifact) will avoid re-uploading the image data and instead store a reference to the original source.

```python
wandb.init()
art = wandb.use_artifact("my_images:latest")
img_1 = art.get(PATH)
wandb.log({"image": img_1})
```
