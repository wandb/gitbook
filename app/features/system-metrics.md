---
description: Metrics automatically logged by wandb
---

# System Metrics

`wandb` automatically logs system metrics every 2 seconds, averaged over a 30 second period. The metrics include:

* CPU Utilization
* System Memory Utilization
* Disk I/O Utilization
* Network traffic \(bytes sent and received\)
* GPU Utilization
* GPU Temperature
* GPU Time Spent Accessing Memory \(as a percentage of the sample time\)
* GPU Memory Allocated

GPU metrics are collected on a per-device basis using [nvidia-ml-py3](https://github.com/nicolargo/nvidia-ml-py3/blob/master/pynvml.py). For more information on how to interpret these metrics and optimize your model's performance, see [this helpful blog post from Lambda Labs](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/).

