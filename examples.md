---
description: >-
  How to use Weights & Biases: snippets, scripts, interactive notebooks, and
  videos.
---

# Examples

Explore what's possible in the W\&B app with the example projects below. Looking for code examples instead? Head to our [GitHub repo](https://github.com/wandb/examples).

1. [Examples by application](examples.md#examples-by-application)
   1. [Autonomous vehicles](examples.md#autonomous-vehicles)
      1. [Visualize LIDAR point clouds of driving scenes](https://wandb.ai/stacey/lyft/reports/LIDAR-Point-Clouds-of-Driving-Scenes--Vmlldzo2MzA5Mg)
      2. [Image Masks for Semantic Segmentation](https://wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)&#x20;
      3. [Bounding Boxes for Object Detection](https://wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)
      4. [Self-Driving Cars learning depth perception](https://wandb.ai/stacey/sfmlearner/reports/Video-to-3D-Depth-Perception-for-Self-Driving-Cars--Vmlldzo2Nzg2Nw)&#x20;
      5. [Deep Drive: Semantic segmentation for scene parsing](https://wandb.ai/stacey/deep-drive/reports/The-View-from-the-Driver-s-Seat--Vmlldzo1MTg5NQ)
   2. [Biomedical](examples.md#biomedical)
      1. [DeepChem: Molecular Solubility](https://wandb.ai/stacey/deepchem\_molsol/reports/DeepChem-Molecular-Solubility--VmlldzoxMjQxMjM)
      2. [3D protein-ligand interactions](https://wandb.ai/stacey/deepchem\_interact/reports/DeepChem-Molecular-Interaction--VmlldzoxMzMxNDE)
      3. [Exploring X-Ray data and long-tailed learning](https://wandb.ai/stacey/xray/reports/X-Ray-Illumination--Vmlldzo4MzA5MQ)
      4. [Logging RDKit Molecular data](https://wandb.ai/anmolmann/rdkit\_molecules/reports/Logging-RDKit-Molecular-Data--VmlldzoxMjk1MjQ1)
   3. [Finance](examples.md#finance)
      1. [Interpretable credit scorecards with XGBoost](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/boosting/Credit\_Scorecards\_with\_XGBoost\_and\_W%26B.ipynb)
2. [Examples by technique](examples.md#examples-by-technique)
   1. [Classification](examples.md#classification)
      1. [Sentiment Classification using JAX/FLAX ](https://www.kaggle.com/heyytanay/sentiment-clf-jax-flax-on-tpus-w-b/notebook)
      2. [Tables tutorial: Visualize Data for Image Classification](https://wandb.ai/stacey/mendeleev/reports/Tables-Tutorial-Visualize-Data-for-Image-Classification--VmlldzozNjE3NjA)
   2. [Computer Vision](examples.md#computer-vision)
      1. [Train and fine-tune CNNs beyond ImageNet](https://wandb.ai/stacey/curr\_learn/reports/Classify-the-Natural-World--Vmlldzo1MjY4Ng)
      2. [Extracting text from visually structured forms](https://wandb.ai/stacey/deepform\_v1/reports/DeepForm-Understand-Structured-Documents-at-Scale--VmlldzoyODQ3Njg)
   3. [Distributed Training](examples.md#distributed-training)
      1. [Data Parallel distributed training in Keras](https://wandb.ai/stacey/estuary/reports/Distributed-Training--Vmlldzo1MjEw)
      2. [Optimize Pytorch-Lightning models](https://www.pytorchlightning.ai/blog/use-pytorch-lightning-with-weights-biases)

## Examples by application

A list of examples by applications to guide you how W\&B is solving common problems.

### Autonomous vehicles

{% tabs %}
{% tab title="Point Clouds" %}
See [LIDAR point cloud visualizations](https://wandb.ai/stacey/lyft/reports/LIDAR-Point-Clouds-of-Driving-Scenes--Vmlldzo2MzA5Mg) from the Lyft dataset. These are interactive and have bounding box annotations. Click the full screen button in the corner of an image, then zoom, rotate, and pan around the 3D scene.

![](<.gitbook/assets/image (130).png>)
{% endtab %}

{% tab title="Segmentation" %}
[This report ](https://wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)describes how to log and interact with image masks for semantic segmentation.

![](<.gitbook/assets/image (112).png>)


{% endtab %}

{% tab title="Bounding Boxes" %}
[Examples & walkthrough](https://wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ) of how to annotate driving scenes for object detection

![](<.gitbook/assets/image (131).png>)
{% endtab %}

{% tab title="3D from Video" %}
[Infer depth perception](https://wandb.ai/stacey/sfmlearner/reports/Video-to-3D-Depth-Perception-for-Self-Driving-Cars--Vmlldzo2Nzg2Nw) from dashboard camera videos. This example contains lots of sample images from road scenes, and shows how to use the media panel for visualizing data in W\&B.

![](<.gitbook/assets/image (111).png>)
{% endtab %}

{% tab title="Deep Drive" %}
[This report](https://wandb.ai/stacey/deep-drive/reports/The-View-from-the-Driver-s-Seat--Vmlldzo1MTg5NQ) compares models for detecting humans in scenes from roads, with lots of charts, images, and notes. [The project page](https://wandb.ai/demo-team/deep-drive?workspace=user-stacey) workspace is also available.

![](<.gitbook/assets/image (129).png>)
{% endtab %}
{% endtabs %}

### Biomedical

{% tabs %}
{% tab title="2D Molecules" %}
[This report ](https://wandb.ai/stacey/deepchem\_molsol/reports/DeepChem-Molecular-Solubility--VmlldzoxMjQxMjM)explores training models to predict [how soluble a molecule](https://wandb.ai/stacey/deepchem\_molsol/reports/DeepChem-Molecular-Solubility--VmlldzoxMjQxMjM) is in water based on its chemical formula. This example features scikit learn and sweeps.

![](<.gitbook/assets/image (121).png>)
{% endtab %}

{% tab title="3D Molecules" %}
[This report](https://wandb.ai/stacey/deepchem\_interact/reports/DeepChem-Molecular-Interaction--VmlldzoxMzMxNDE) explores molecular binding and shows interactive 3D protein visualizations.

![](<.gitbook/assets/image (122).png>)
{% endtab %}

{% tab title="X Rays" %}
[This report ](https://wandb.ai/stacey/xray/reports/X-Ray-Illumination--Vmlldzo4MzA5MQ)explores chest x-ray data and strategies for handling real world long-tailed data.

![](<.gitbook/assets/image (100).png>)
{% endtab %}

{% tab title="RDKit" %}
[This report](https://wandb.ai/anmolmann/rdkit\_molecules/reports/Logging-RDKit-Molecular-Data--VmlldzoxMjk1MjQ1) explores `rdkit` feature for logging molecular data.

[Click here](https://wandb.ai/anmolmann/rdkit\_molecules) to view and interact with a live W\&B Dashboard built with this [notebook](http://wandb.me/rdkit).

{% embed url="https://user-images.githubusercontent.com/7557205/144367246-cc052e58-ede4-4374-9307-4f185328c361.gif" %}
{% endtab %}
{% endtabs %}

### Finance

{% tabs %}
{% tab title="Credit Scorecards" %}
Track experiments, generate credit scorecard for loan defaults and run a hyperparameter sweep to find the best hyperparameters. [Click here](https://wandb.ai/morgan/credit\_scorecard) to view and interact with a live W\&B Dashboard built with [this](http://wandb.me/xgboost) notebook.

![](<.gitbook/assets/image (165) (1) (1).png>)
{% endtab %}
{% endtabs %}

## Examples by technique

### Classification

{% tabs %}
{% tab title="Sentiment Classification" %}
[Sentiment Classification](https://www.kaggle.com/heyytanay/sentiment-clf-jax-flax-on-tpus-w-b/notebook) on 1.6 Million tweets using Jax/Flax with TPUs using HuggingFace BERT and W\&B Tracking!

![Sentiment Classifcation using JAX/FLAX](<.gitbook/assets/image (163).png>)
{% endtab %}

{% tab title="Image Classification" %}
Read [this report](https://wandb.ai/stacey/mendeleev/reports/Visualize-Data-for-Image-Classification--VmlldzozNjE3NjA), follow [this colab](https://wandb.me/dsviz-nature-colab), or explore this [artifacts context](https://wandb.ai/stacey/mendeleev/artifacts/val\_epoch\_preds/val\_pred\_gawf9z8j/2dcee8fa22863317472b/files/val\_epoch\_res.table.json) for a CNN identifying 10 types of living things (plants, bird, insects, etc) from [iNaturalist](https://www.inaturalist.org/pages/developers) photos.

![Compare the distribution of true labels across two different models' predictions.](<.gitbook/assets/image (161).png>)
{% endtab %}
{% endtabs %}

### **Computer Vision**

{% tabs %}
{% tab title="Images of species" %}
[This report](https://wandb.ai/stacey/curr\_learn/reports/Classify-the-Natural-World--Vmlldzo1MjY4Ng) explores per-class accuracy on an image dataset of plants and animals.

![](<.gitbook/assets/image (80).png>)

![](<.gitbook/assets/image (85).png>)
{% endtab %}

{% tab title="PDF scans" %}
[Parse TV ad receipts for political campaigns](https://wandb.ai/stacey/deepform\_v1/reports/DeepForm-Understand-Structured-Documents-at-Scale--VmlldzoyODQ3Njg) to extract amount paid, organization, dates of ad, and receipt id from 100s of different receipt formats.

![](<.gitbook/assets/image (133).png>)
{% endtab %}
{% endtabs %}

### Distributed Training

{% tabs %}
{% tab title="Data Parallel" %}
[This report ](https://wandb.ai/stacey/estuary/reports/Distributed-Training--Vmlldzo1MjEw)visualizes experiments with Keras data parallel across up to 8 GPUs. Features include run sets and grouping, and notes.

![](<.gitbook/assets/image (88).png>)


{% endtab %}
{% endtabs %}
