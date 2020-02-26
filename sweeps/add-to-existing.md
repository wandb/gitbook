---
description: >-
  If you're already using wandb.init, wandb.config, and wandb.log in your
  project, start here!
---

# Create Sweep from existing W&B project

If you have an existing W&B project, it’s easy to start exploring models with hyperparameter sweeps. In this doc we'll cover:

1. How to create a sweep from the UI
2. How to feed in existing runs to seed a new sweep

## Create a sweep from the UI

It's fast and easy to start a new sweep from the project page. Open the [example project](https://app.wandb.ai/carey/pytorch-cnn-fashion) and [example code](https://github.com/wandb/examples/tree/master/pytorch-cnn-fashion) to follow along with this tutorial.

Start from the project page. Here you can see a couple of previous runs that I trained manually.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579062396185_image.png)

Open the sweep tab and click “Create sweep” in the upper right corner.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579062673820_image.png)

These steps take me through running my first sweep. To make sure I have the latest version of `wandb` I run `pip install --upgrade wandb` first.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579062719835_image.png)

The auto-generated config guesses values to sweep over based on the runs I’ve done already. In the “Parameters” tab, I remove **channels\_one** and **channels\_two** from my sweep config. I don’t want to sweep over those hyperparameters. Once I’m happy with the ranges of parameters to sweep over, I download the file.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579062904325_image.png)

I move the generated config file to my training script repo.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1578430062927_sweep+yaml+move.png)

I run `wandb sweep sweep.yaml` to start a sweep on the W&B server. This is a centralized service sends out the next set of hyperparameters to agents that I run on my own machines.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579063801261_image.png)

Next I launch an agent locally. I can launch dozens of agents in parallel if I want to distribute the work and finish the sweep more quickly. The agent will print out the set of parameters it’s trying next.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579063895733_image.png)

That’s it! Now I’m running a sweep. Here’s what [my dashboard](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q) looks like as the sweep begins to explore the space of hyperparameters.

![](https://paper-attachments.dropbox.com/s_5D8914551A6C0AABCD5718091305DD3B64FFBA192205DD7B3C90EC93F4002090_1579066494222_image.png)

## Seed a new sweep with existing runs

Launch a new sweep using existing runs that you've previously logged.

1. Open your project table.
2. Select the runs you want to use with checkboxes on the left side of the table.
3. Click the dropdown to create a new sweep.

![](../.gitbook/assets/image%20%2822%29.png)

Your sweep will now be set up on our server. All you need to do is launch one or more agent to start running runs.

![](../.gitbook/assets/image%20%2832%29.png)

Before you launch the run you can still edit the config. Scroll down in the Sweep Overview tab to see the config, and you can click the link to download your configuration file and save it locally in your project directory. Then you need to run the `wandb sweep --update SWEEP_ID sweep.yaml` command to update the sweep config on our server, so when your agents ask for the next set of hyperparameters we're giving them the right ones.

![](../.gitbook/assets/image%20%2842%29.png)

