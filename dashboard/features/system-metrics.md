---
description: wandb自动记录的指标
---

# System Metrics

 `wandb`每2秒自动记录一次系统指标，平均30秒一次。这些指标包括：

* CPU使用率
* 系统内存使用情况
* 磁盘I/O利用率
* 网络流量\(发送和接收的字节数\)
* GPU利用率
* GPU温度
* GPU访问内存所花费的时间（占采样时间的百分比）
* GPU内存分配

GPU 指标是使用 [nvidia-ml-py3 按](https://github.com/nicolargo/nvidia-ml-py3/blob/master/pynvml.py)每个设备收集的。有关如何解释这些指标和优化模型性能的更多信息，请参见 [Lambda Labs 的这篇有用的博客文章](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)。

