# Company

### What is wandb?

Wandb is an experiment tracking tool for machine learning. We make it easy for anyone doing machine learning to keep track of experiments and share results with colleagues and their future self.

Here's a 1 minute overview video. [View an example project →](https://app.wandb.ai/stacey/estuary)

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" %}

### How does it work?

When you instrument your training code with wandb, our background process will collect useful data about what is happening  as you train your models. For example, we can track model performance metrics, hyperparameters, gradients, system metrics, output files, and your most recent git commit.

{% page-ref page="../ref/export-api/examples.md" %}

### How hard is it to set up?

We know that most people track their training with tools like emacs and Google Sheets, so we've designed wandb to be as lightweight as possible. Integration should take 5-10 minutes, and wandb will not slow down or crash your training script.

## Benefits of wandb

Our users tell us they get three kinds of benefits from wandb:

### 1. Visualizing training

Some of our users think of wandb as a "persistent TensorBoard". By default, we collect model performance metrics like accuracy and loss. We can also collect and display matplotlib objects, model files, system metrics like GPU usage, and your most recent git commit SHA + a patch file of any changes since your last commit.

You can also take notes about individual runs to be saved along with your training data. Here's a relevant [example project](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview) from a class we taught to Bloomberg.

### 2. Organize and compare lots of training runs

Most people training machine learning models are trying lots and lots of versions of their model and our goal is to help people stay organized.

You can create projects to keep all of your runs in a single place. You can visualize performance metrics across lots of runs and filter, group, and tag them any way you like.

A good example project is Stacey's [estuary project](https://app.wandb.ai/stacey/estuary). In the sidebar you can turn on and off runs to show on the graphs, or click one run to dive deeper. All your runs get saved and organized in a unified workspace for you.

![](../.gitbook/assets/image%20%2884%29.png)

### 3. Share your results

Once you have done lots of runs you usually want to organize them to show some kind of result. Our friends at Latent Space wrote a nice article called [ML Best Practices: Test Driven Development](https://www.wandb.com/articles/ml-best-practices-test-driven-development) that talks about how they use W&B reports to improve their team's productivity.

A user Boris Dayma wrote a public example report on [Semantic Segmentation](https://app.wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report). He walks through various approaches he tried and how well they work.

We really hope that wandb encourages ML teams to collaborate more productively.

If you want to learn more about how teams use wandb, we've recorded interviews with our technical users at [OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai) and [Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8).

## Teams

If you're working on a machine learning project with collaborators, we make it easy to share results.

* [Enterprise Teams](https://www.wandb.com/pricing): We support small startups and large enterprise teams like OpenAI and Toyota Research Institute. We have flexible pricing options to fit your team's needs, and we support hosted cloud, private cloud, and on-prem installations.
* [Academic Teams](https://www.wandb.com/academic): We are dedicated to supporting the academic transparent and collaborative research. If you're an academic, we will grant you access to free teams to share your research in private projects.

If you'd like to share a project with people outside a team, click on the project privacy settings in the navigation bar and set the project to "Public." Anyone you share a link with will be able to see your public project results.

