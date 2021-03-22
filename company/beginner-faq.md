---
description: Resources for people getting started with Machine Learning
---

# Beginner FAQ

## Are you learning machine learning?

Wandb is a tool for visualizing training and we hope that it's useful for everyone from experts to people just getting started. If you have general questions about machine learning you are welcome to ask them in our [Slack](http://wandb.me/slack) channel. We have also made some free [tutorial videos](https://www.wandb.com/tutorials) with example code that are designed to get you started.

One great way to learn machine learning is to start with an interesting project. If you don't have a project in mind, a good place to find projects is our [benchmarks](https://www.wandb.com/benchmarks) page - we have a variety of machine learning tasks with data and working code that you can improve.

## Online Resources

There are a lot of excellent online resources for learning machine learning. Please send us a note if we should add anything here.

* [fast.ai ](https://www.fast.ai)- Excellent practical machine learning classes and friendly community.
* [deep learning book](http://www.deeplearningbook.org) - Detailed book available for free online.
* [Stanford CS229](https://see.stanford.edu/Course/CS229) - Lectures from a great class available online.

## Looking for bias in models

If you're training a machine learning model, you want to be able to visualize how it performs on different inputs. A common problem, especially when you're getting started, is that it's hard to get those visualizations set up. That's where Weights & Biases comes in. We make it easy to get metrics to understand your model performance.

Here's a hypothetical example— you're training a model to identify objects on the road. Your dataset is a bunch of labeled images with cars, pedestrians, bicycles, trees, buildings, etc. As you train your model, you can visualize the different class accuracies. That means you can see if your model is great at finding cars but bad at finding pedestrians. This could be a dangerous bias, especially in a self-driving car model.

Interested in seeing a live example? Here's a report that compares the model's accuracy on identifying images of different types of plants and animals— birds, mammals, fungi etc. The Weights & Biases graphs make it easy to see how each version of the model \(each line on the graph\) performs on different classes.

[See the report in W&B →](https://app.wandb.ai/stacey/curr_learn/reports/Species-Identification--VmlldzoxMDk3Nw)

![](../.gitbook/assets/image%20%2818%29%20%283%29%20%283%29.png)

