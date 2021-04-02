---
description: 可视化模型的超参数和输出指标之间的关系。
---

# Parameter Importance

该面板显示了你的超参数中哪些是你的指标的最佳预测因素，并且与你的指标的理想值高度相关。

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795733856_image.png)

**相关性**是指超参数与所选指标（本例中为val\_loss）之间的线性相关性。因此，高相关性意味着当超参数具有较高的值时，指标也具有较高的值，反之亦然。相关性是一个很好的指标，但它不能捕捉输入之间的二阶交互作用，而且比较范围迥异的输入可能会变得混乱。

因此我们还计算一个**重要性**指标，我们以超参数为输入，以指标为目标输出，训练一个随机森林（Random Forest），并报告随机森林的重要性值。

这个技术的想法是受与[Jeremy Howard的](https://twitter.com/jeremyphoward)对话启发，他在[Fast.ai](http://fast.ai/)率先使用随机森林特征重要性来探索超参数空间。我们强烈建议你查看他非凡的[讲座](http://course18.fast.ai/lessonsml1/lesson4.html)（以及这些[笔记](https://forums.fast.ai/t/wiki-lesson-thread-lesson-4/7540)），以了解更多关于这种分析背后的驱动力。

此超参数重要性面板可解开高度相关的超参数之间复杂的相互作用。这样，它可以帮助你微调你的超参数搜索，向你展示哪些超参数对预测模型性能最重要。

## **创建超参数重要性面板** <a id="creating-a-hyperparameter-importance-panel"></a>

转到你的Weights & Biases 项目。如果你没有，你可以使用[这个项目](https://app.wandb.ai/sweep/simpsons)。

从你的项目页面，单击**添加可视化**。

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795570241_image.png)

然后选择"**参数重要性"**。

除了[将Weights & Biases集成](https://docs.wandb.com/quickstart)到你的项目中，你不需要编写任何代码。

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578795636072_image.png)

## **解释超参数重要性面板** <a id="using-the-hyperparameter-importance-panel"></a>

 这个面板显示了在你的训练脚本中传递给[wandb.config](https://docs.wandb.com/library/python/config)对象的所有参数。接下来，它显示了这些配置参数与你选择的模型指标（本例中为val\_loss）的功能的重要性和相关性。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVteUSm-FWB88LKEuD2%2F-MVtietPjlxN_LLyMkwl%2F2021-03-16%2011.58.56.gif?alt=media&token=fde8634b-0beb-4edd-ba69-2e989ff88de7)

除了[将Weights & Biases集成](https://docs.wandb.com/quickstart)到你的项目中，你不需要编写任何代码。

![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MVteUSm-FWB88LKEuD2%2F-MVtl2rLmF4mVzMYrypD%2F2021-03-16%2012.09.57.gif?alt=media&token=2d91a533-a05b-4e2c-b051-79a97b51e8f7)

### **解释超参数重要性面板**

![](https://paper-attachments.dropbox.com/s_B78AACEDFC4B6CE0BF245AA5C54750B01173E5A39173E03BE6F3ACF776A01267_1578798509642_image.png)

这个面板显示了在你的训练脚本中传递给[wandb.config](https://docs.wandb.com/library/python/config)对象的所有参数。接下来，它显示了这些配置参数与你选择的模型指标（本例中为`val_loss`）的功能的重要性和相关性。

### **重要性** <a id="importance"></a>

重要性一栏显示了每个超参数对预测所选指标的有用程度。我们可以想象一下这样的场景：我们首先调整大量的超参数，并利用这个图来磨合哪些超参数值得进一步探索。然后，后续的扫描可以限制在最重要的超参数上，从而更快、更低成本地找到更好的模型。

注：我们使用基于树的模型而不是线性模型来计算这些重要性，因为前者对分类数据和未归一化的数据的容忍度更高。在上述面板中，我们可以看到，`epochs`、`learning_rate`、`batch_size`和`weight_decay`相当重要。

 作为下一步，我们可能会运行另一个扫描，探索这些超参数的更细粒度的值。有趣的是，虽然`learning_rate`和`batch_size`很重要，但它们与输出的相关性并不高。接下来了解相关性。

### **相关性** <a id="correlations"></a>

相关性捕获单个超参数和指标值之间的线性关系。它们回答了一个问题--使用一个超参数，比如SGD优化器，和我的val\_loss之间是否有显著的关系（本例中的答案是肯定的）。相关性值的范围从-1到1，其中正值代表正线性相关，负值代表负线性相关，值为0代表没有相关。一般来说，任何一个方向的值大于0.7都代表强相关。

我们可以使用这个图来进一步探索与我们的指标有更高相关性的值（在这种情况下，我们可能会选择随机梯度下降法或adam而不是rmsprop或nadam），或者训练更多的周期。

 关于解释相关性的快速说明:

* 相关性显示了相关联的迹象，而不一定是因果关系。
* 相关性对异常值很敏感，这可能会把一个强关系变成一个中等的关系，特别是当尝试的超参数的样本量很小时。
* 最后，相关性只能捕捉超参数和指标之间的线性关系。如果有很强的多项式关系，就不会被相关性捕获。

重要性和相关性之间的差异是由于重要性考虑了超参数之间的相互作用，而相关性只衡量单个超参数对指标值的影响。其次，相关性只捕捉到线性关系，而重要性可以捕捉到更复杂的关系。

正如你所看到的，重要性和相关性都是了解超参数如何影响模型性能的强大工具。

我们希望这个面板能帮助你更快地捕捉到这些洞察力，并磨合出一个强大的模型。[  
](https://docs.wandb.ai/app/features/panels/compare-metrics/smoothing)

