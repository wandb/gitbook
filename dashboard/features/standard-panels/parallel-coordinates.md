---
description: 在机器学习实验中可视化高维数据
---

# Parallel Coordinates

这是平行坐标图的示例。每个轴代表一个不同的超参数。在这个例子中我选择了四个垂直轴。在这种情况下，我正在可视化不同超参数和我的模型的最终精度之间的关系。

* **轴**: 来自 [wandb.config](https://docs.wandb.ai/v/zh-hans/library/wandb.config) 的不同超参数和来自 [wandb.log\(\) ](https://docs.wandb.ai/v/zh-hans/library/wandb.log)的指标​
* **线**: 每条线代表一次运行。将鼠标悬停在一条线上可查看包含运行详细信息的工具提示。与当前过滤器匹配的所有线条都会显示，但如果您关闭眼睛，线条将变灰。

**面板设置**

在面板设置中配置这些功能 —— 单击面板右上角的编辑按钮。

* **工具提示**: 悬停时，图例会显示每次运行的信息
* **标题**: 编辑轴标题以提高可读性
* **渐变**: 自定义渐变为您喜欢的任何颜色范围
* **对数刻度**: 可以将每个轴独立设置为在对数刻度上查看
* **翻转轴**: 切换轴方向——这在您将精度和损失都作为列时很有用

  [实时观看 →​](https://app.wandb.ai/example-team/sweep-demo/reports/Zoom-in-on-Parallel-Coordinates-Charts--Vmlldzo5MTQ4Nw)

[  
](https://docs.wandb.ai/app/features/panels/code)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M5xuU0oX-LvRy7dSwR6%2F-M5xukhBPmcayMQ4aM91%2F2020-04-27%2016.11.43.gif?alt=media&token=f3e78351-3eff-4137-a9a3-0dc30355165c)

