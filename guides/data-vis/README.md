---
description: Iterate on datasets and understand model predictions
---

# Data Visualization

Use **W\&B Tables** to log, query, and analyze tabular data. Understand your datasets, visualize model predictions, and share insights in a central dashboard.

* Compare changes precisely across models, epochs, or individual examples
* Understand higher-level patterns in your data
* Capture and communicate your insights with visual samples

## Quickly log your first table

The fastest way to try Tables is to log a dataframe and see the Table UI.

```python
wandb.log({"table": my_dataframe})
```

![Directly log a dataframe to get a Table](<../../.gitbook/assets/wandb - iris table (1).png>)

### Rich media

Add rich media to your logged [Table](log-tables.md) (images, audio, point clouds, etc) with `wandb` [data types](../../ref/python/data-types/). 

![](<../../.gitbook/assets/wandb - demo table visualizer.png>)

A W\&B Table (`wandb.Table`) is a two dimensional grid of data where each column has a single type of data—think of this as a more powerful DataFrame. Tables support primitive and numeric types, as well as nested lists, dictionaries, and rich media types. Log a Table to W\&B, then query, compare, and analyze results in the UI.

Tables are great for storing, understanding, and sharing any form of data critical to your ML workflow—from datasets to model predictions and everything in between.

## Why use Tables?

### **Actually see your data**

Log metrics and rich media during model training or evaluation, then visualize results in a persistent database synced to the cloud, or to your [self-hosted instance](https://docs.wandb.ai/guides/self-hosted). For example, check out this [balanced split of a photos dataset →](https://wandb.ai/stacey/mendeleev/artifacts/balanced_data/inat\_80-10-10\_5K/ab79f01e007113280018/files/data_split.table.json)

![Browse actual examples and verify the counts & distribution of your data](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZVv0k-vSjItkYUQiex%2F-MZYHUyVTRhOR1SSW-Qb%2FScreen%20Shot%202021-04-30%20at%207.57.22%20AM.png?alt=media\&token=9634825b-4b3b-42cf-a19b-7d2338384a06)

### **Interactively explore your data**

View, sort, filter, group, join, and query Tables to understand your data and model performance—no need to browse static files or rerun analysis scripts. For example, see this project on [style-transfered audio →](https://wandb.ai/stacey/cshanty/reports/Whale2Song-W-B-Tables-for-Audio--Vmlldzo4NDI3NzM)

![Listen to original songs and their synthesized versions (with timbre transfer)](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZVW5FKKkNnlhtrUDki%2F-MZVtzeY7yxIKqX1EWZZ%2FScreen%20Shot%202021-04-29%20at%208.52.10%20PM.png?alt=media\&token=4ef6f175-cb0d-40e6-aaa5-a8dc30236fcb)

### **Compare model versions**

Quickly compare results across different training epochs, datasets, hyperparameter choices, model architectures etc. For example, take a look at this comparison of [two models on the same test images →](https://wandb.ai/stacey/evalserver_answers\_2/artifacts/results/eval_Daenerys/c2290abd3d7274f00ad8/files/eval_results.table.json#b6dae62d4f00d31eeebf$eval_Bob)

![See granular differences: the left model detects some red sidewalk, the right does not.](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZVW5FKKkNnlhtrUDki%2F-MZVuqE2fGRN2W40xpVh%2FScreen%20Shot%202021-04-29%20at%208.55.25%20PM.png?alt=media\&token=e83f6492-a6f2-49a4-9e08-9de9a1612567)

### **Track every detail and see the bigger picture**

Zoom in to visualize a specific prediction at a specific step. Zoom out to see the aggregate statistics, identify patterns of errors, and understand opportunities for improvement. This tool works for comparing steps from a single model training, or results across different model versions. Check out this example table analyzing results [after 1 vs 5 epochs on MNIST →](https://wandb.ai/stacey/mnist-viz/artifacts/predictions/baseline/d888bc05719667811b23/files/predictions.table.json#7dd0cd845c0edb469dec)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZVv0k-vSjItkYUQiex%2F-MZYFWL-jOLCPReRojpD%2FScreen%20Shot%202021-04-30%20at%207.49.27%20AM.png?alt=media\&token=1413ae8b-6db7-4ba6-9b77-2c2ef640a408)

## Example Projects with W\&B Tables

### Image classification

Read [this report](https://wandb.ai/stacey/mendeleev/reports/Visualize-Data-for-Image-Classification--VmlldzozNjE3NjA), follow [this colab](https://wandb.me/dsviz-nature-colab), or explore this [artifacts context](https://wandb.ai/stacey/mendeleev/artifacts/val_epoch_preds/val_pred_gawf9z8j/2dcee8fa22863317472b/files/val_epoch_res.table.json) for a CNN identifying 10 types of living things (plants, bird, insects, etc) from [iNaturalist](https://www.inaturalist.org/pages/developers) photos.

![Compare the distribution of true labels across two different models' predictions.](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZZ-rHMOwjfrB1rXLg9%2F-MZZ404GQuSIDUt0ilCG%2FScreen%20Shot%202021-04-30%20at%2011.37.23%20AM.png?alt=media\&token=8290d47a-4de2-4441-a389-0a8c39135102)

### Audio

Interact with audio Tables in[ this report](https://wandb.ai/stacey/cshanty/reports/Whale2Song-W-B-Tables-for-Audio--Vmlldzo4NDI3NzM) on timbre transfer. You can compare a recorded whale song with a synthesized rendition of the same melody on an instrument like violin or trumpet. You can also record your own songs and explore their synthesized versions in W\&B via [this colab → ](http://wandb.me/audio-transfer)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZZ4TzdR0JwQNM7nIKD%2F-MZZAmcG-4OfHGOxnA38%2FScreen%20Shot%202021-04-30%20at%2012.08.52%20PM.png?alt=media\&token=bfea3626-a447-4f40-823b-e2072ef1ae89)

### Text

Browse text samples from training data or generated output, dynamically group by relevant fields, and align your evaluation across model variants or experiment settings. Explore a simple character-based RNN for generating Shakespeare in [this report →](https://wandb.ai/stacey/nlg/reports/Visualize-Text-Data-Predictions--Vmlldzo1NzcwNzY)

![Doubling the size of the hidden layer yields some more creative prompt completions.](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZovIldq3JZifxZ9pSh%2F-MZoyefoDx_TwzrWxIv7%2Fshakesamples.png?alt=media\&token=7323f9e4-f565-43f2-8a3e-43c9b3d38550)

### Video

Browse and aggregate over videos logged during training to understand your models. Here is an early example using the [SafeLife benchmark](https://wandb.ai/safelife/v1dot2/benchmark) for RL agents seeking to [minimize side effects →](https://wandb.ai/stacey/saferlife/artifacts/video/videos_append-spawn/c1f92c6e27fa0725c154/files/video_examples.table.json)

![Browse easily through the few successful agents](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZovIldq3JZifxZ9pSh%2F-MZp-5h62yXSoqrCOJYC%2FScreen%20Shot%202021-04-22%20at%209.41.27%20PM.png?alt=media\&token=56439234-ab05-47fa-8c2c-38b22de8339f)

### Comparing model variants (semantic segmentation)

An [interactive notebook](https://wandb.me/dsviz-cars-demo) and [live example](https://wandb.ai/stacey/evalserver_answers\_2/artifacts/results/eval_Daenerys/c2290abd3d7274f00ad8/files/eval_results.table.json#a57f8e412329727038c2$eval_Ada) of logging Tables for semantic segmentation and comparing different models. Try your own queries [in this Table →](https://wandb.ai/stacey/evalserver_answers\_2/artifacts/results/eval_Daenerys/c2290abd3d7274f00ad8/files/eval_results.table.json)

![Find the best predictions across two models on the same test set](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MZovIldq3JZifxZ9pSh%2F-MZp1p-1yi0JtkF5VAtu%2FScreen%20Shot%202021-05-03%20at%206.41.36%20PM.png?alt=media\&token=6261dee5-7bb7-412b-befd-a9ae27856401)

### Analyzing improvement over training time

A detailed report on [visualizing predictions over time](https://wandb.ai/stacey/mnist-viz/reports/Visualize-Predictions-over-Time--Vmlldzo1OTQxMTk) and the accompanying [interactive notebook →](https://wandb.me/dsviz-mnist-colab)
