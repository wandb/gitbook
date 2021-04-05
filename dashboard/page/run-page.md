---
description: 你的模型的每一个训练运行会获得一个独立页面，被组织在更大的项目中。
---

# Run Page

使用运行页面来探究你的模型的单个版本的详细信息。

## **概览选项卡** <a id="overview-tab"></a>

* 运行名称、描述和标签
* 主机名称、操作系统、Python版本和启动运行的命令。
* 用[wandb.config](https://docs.wandb.ai/library/config)保存的配置参数列表​
* 用[wandb.log\(\)](https://docs.wandb.ai/library/log)保存的总结参数列表，默认设置为最后一个记录的值。

​ [查看实战案例 →](https://wandb.ai/carey/pytorch-cnn-fashion/runs/munu5vvg/overview?workspace=user-carey) W&B仪表盘运行概览选项卡​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_JlUtDfjZNjvUA6hw%2Fwandb%20run%20overview%20page.png?alt=media&token=5a61d575-778c-4435-88e1-eaf9abfd5fdc)

即使你将该页面本身公开，Python的细节也是私有的。下面以我的运行页面为例，左边是匿名模式，右边是我的账号。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-M4K5z9XFKK4j-RLEz6J%2F-M4K75wP2mopixCncWmL%2FScreen%20Shot%202020-04-07%20at%207.46.39%20AM.png?alt=media&token=83298265-3d5d-4061-b2d8-82513c7bda77)

## **图表选项卡** <a id="charts-tab"></a>

* 搜索、分组和排列可视化
* 点击图像上的铅笔图标✏️ 进行编辑
  * 修改x轴、指标（Metric）和范围
  * 编辑图表的图例、标题和颜色
* 查看验证集中的示例预测
* 要获得这些图表，请使用[wandb.log\(\)](https://docs.wandb.ai/library/log)记录数据​

​[查看实战案例](https://wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)[ →](https://app.wandb.ai/wandb/examples-keras-cnn-fashion/runs/wec25l0q?workspace=user-carey)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_KKTDaaOER0LmNm46%2Fwandb%20run%20page%20workspace%20tab.png?alt=media&token=8fc8eb30-eb4e-4667-a7bd-e06e295fa719)

## **系统选项卡** <a id="system-tab"></a>

* 可视化CPU利用率、系统内存、磁盘I/O、网络流量、GPU利用率、GPU温度、GPU访问内存所花费的时间、GPU内存分配和GPU功率使用情况。
* Lambda Labs在一篇[博客](https://lambdalabs.com/blog/weights-and-bias-gpu-cpu-utilization/)中强调了如何使用W&B系统指标​

 [查看实战案例](https://wandb.ai/stacey/deep-drive/runs/ki2biuqy/system?workspace=user-carey)[→](https://wandb.ai/stacey/deep-drive/runs/ki2biuqy/system?workspace=user-carey)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_KmFRJlbUnXSYdb_j%2Fwandb%20system%20utilization.png?alt=media&token=65c67e63-aa2b-4c25-887f-40be68acda89)

## **模型选项卡** <a id="model-tab"></a>

* 查看你的模型的层、参数数量以及每个的输出形状 

​ [查看实战案例](https://wandb.ai/stacey/deep-drive/runs/pr0os44x/model)[ →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/model)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_LAmOBDxNF_RXdYM9%2Fwandb%20run%20page%20model%20tab.png?alt=media&token=36a79074-f396-48de-b74f-975d2b6fe382)

##  **日志选项卡** <a id="logs-tab"></a>

* 命令行上打印的输出，训练模型的机器的标准输出（stdout）和标准错误（stderr）
* 我们显示最后的1000行。运行结束后，如果你想下载完整的日志文件，请点击右上角的下载按钮。        

[  
查看实战案例 →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/logs)​​

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_LTV7zTJnllO6vVwU%2Fwandb%20run%20page%20log%20tab.png?alt=media&token=695e122f-3469-4c57-8226-ab60f0d41886)

## **文件选项卡** <a id="files-tab"></a>

* 使用[wandb.save\(\)](https://docs.wandb.ai/library/save)保存文件与运行同步​
* 保存模型检查点、验证集示例等
* 使用diff.patch来[恢复](https://docs.wandb.ai/library/restore)你的代码的指定版本

🌟新推荐：尝试用 [Artifacts](https://docs.wandb.ai/artifacts)跟踪输入和输出

​ [查看实战案例 →](https://app.wandb.ai/stacey/deep-drive/runs/pr0os44x/files/media/images)​​[  
](https://docs.wandb.ai/app/pages/workspaces)

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MO_JUEOdC3E22018biz%2F-MO_L_zQWo7zBNG63Jse%2Fwandb%20run%20page%20files%20tab.png?alt=media&token=6b7ea19e-e28d-4f83-aa2e-e5e96c780df5)

