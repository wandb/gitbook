---
description: 超参数搜索和模型优化
---

# Sweeps

使用“权阈”\(Weights & Biases\)扫描器自动执行超参数优化并探索可能的模型。

## **使用W＆B Sweeps的好处**

1. **快速设置**：仅需几行代码，即可运行W＆B sweep。
2. **透明**：我们引用了所有正在使用的算法，并且[我们的代码是开源的](https://github.com/wandb/client/tree/master/wandb/sweeps)。
3. **强大：**我们的sweep完全可定制和配置。您可以在数十台计算机上启动扫描，就像在笔记本电脑上启动扫描一样简单。

## **常见用例**

1. **探索**：有效地采样超参数组合的空间，以发现有希望的区域，建立有关模型的直觉。
2. **优化**：使用sweep查找一组具有最佳性能的超参数。
3. **K-fold cross validation**: K-fold交叉验证：这是使用W＆B Sweeps进行K-fold交叉[验证的](https://github.com/wandb/examples/tree/master/examples/wandb-sweeps/sweeps-cross-validation)简短代码示例。

## **方法**

1. **添加wandb**：在Python脚本中，添加几行代码来记录脚本的超参数和输出度量。[现在开始→](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MKaPhwzNIegNuInaekR/sweeps/quickstart)
2. **写配置：**定义要扫描的变量和范围。选择搜索策略——我们支持网格搜索\(grid search\)，随机搜索\(random search\)和贝叶斯搜索\(Bayesian search\)以及早期停止。在此[处查](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion)看一些示例配置。
3. **初始化扫描器**：启动扫描服务器。我们管理此中央控制器，并在执行扫描的代理之间进行协调。
4. **启动代理：**在要用于扫描中的每台计算机上运行此命令。代理询问中央扫描服务器接下来要尝试哪些超参数，然后执行运行。
5. **可视化结果**：打开实时仪表板，在一个中央位置查看所有结果。

![](../../.gitbook/assets/central-sweep-server-3%20%281%29.png)

