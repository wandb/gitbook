# Company FAQ

## Pricing and Account Plans

W&B is free for individuals and academic teams. We are committed to staying free to support the advancement of machine learning, and we make it easy to export data with our [Export API](../ref/export-api/).

* [Pricing Page](https://www.wandb.com/pricing): Accounts for individuals, startups, teams, and enterprises
* [Academics](https://www.wandb.com/academic): Apply for access to academic teams to share results with collaborators
* [On-prem](../self-hosted/): We have options for on-prem and private cloud installations
* [Contact](getting-help.md): Reach out with any questions
* [Request a demo](https://www.wandb.com/contact): We're happy to get on a call and talk about custom enterprise plans 

## How is Weights & Biases different from TensorBoard?

We were inspired to improve experiment tracking tools for everyone. When the cofounders started working on W&B, they were inspired to build a tool for the frustrated TensorBoard users at OpenAI. Here are a few things we focused on improving:

1. **Reproduce models**: Weights & Biases is good for experimentation, exploration, and reproducing models later. We capture not just the metrics, but also the hyperparameters and version of the code, and we can save your model checkpoints for you so your project is reproducible. 
2. **Automatic organization**: If you hand off a project to a collaborator or take a vacation, W&B makes it easy to see all the models you've tried so you're not wasting hours re-running old experiments.
3. **Fast, flexible integration**: Add W&B to your project in 5 minutes. Install our free open-source Python package and add a couple of lines to your code, and every time you run your model you'll have nice logged metrics and records.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5. **Powerful table**: Search, filter, sort, and group results from different models. It's easy to look over thousands of model versions and find the best performing models for different tasks. TensorBoard isn't built to work well on large projects.
6. **Tools for collaboration**: Use W&B to organize complex machine learning projects. It's easy to share a link to W&B, and you can use private teams to have everyone sending results to a shared project. We also support collaboration via reports— add interactive visualizations and describe your work in markdown. This is a great way to keep a work log, share findings with your supervisor, or present findings to your lab.

Get started with a [free personal account →](http://app.wandb.ai/)

## Who has rights to the data?

You can always export and delete your data at any time. We will never share data associated with private projects. We hope that when you can, you will make your work public so that other practitioners can learn from it.

We hope to discover and share high level patterns to move the field of machine learning forward. For example, we wrote [this article](https://www.wandb.com/articles/monitor-improve-gpu-usage-for-model-training) on how people are not fully utilizing their GPUs. We want to do this in a way that respects your privacy and feels honest. If you have any concerns about data privacy, we'd love to hear from you. Reach out at contact@wandb.com.

## Why are you building these tools?

At Weights & Biases our mission is to build the best tools for machine learning. Our experienced technical cofounders built Figure Eight, and our tools are being used by cutting-edge machine learning teams including OpenAI and Toyota. We enjoy making useful tools, and the best part of our day is interacting with people who are building real models using our product.

![](../.gitbook/assets/image%20%2856%29.png)

## How do I pronounce "wandb"?

You can pronounce it w-and-b \(as we originally intended\), wand-b \(because it's magic like a wand\), or wan-db \(because it saves things like a database\).

## How do I cite Weights & Biases in a paper?

We wrote a [white paper](https://www.dropbox.com/s/0ipub9ewwkml8jf/Experiment%20Tracking%20with%20Weights%20%26%20Biases.pdf?dl=1), and if you're writing a paper or article about a project that used our tools, we'd love it if you cited us. Here's a generated BibTeX citation for our website that will help point people to our tools.

```text
@misc{wandb, 
title = {Experiment Tracking with Weights and Biases}, 
year = {2020}, 
note = {Software available from wandb.com}, 
url={https://www.wandb.com/}, 
author = {Biewald, Lukas},
}
```

