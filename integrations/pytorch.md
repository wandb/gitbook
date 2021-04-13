# Pytorch

W&B为PyTorch提供了一流的支持。要自动记录梯度（Gradient）并存储网络拓扑，你可以调用`watch`并传入你的PyTorch 模型。

```python
import wandb
wandb.init(config=args)

# Magic
wandb.watch(model)

model.train()
for batch_idx, (data, target) in enumerate(train_loader):
    output = model(data)
    loss = F.nll_loss(output, target)
    loss.backward()
    optimizer.step()
    if batch_idx % args.log_interval == 0:
        wandb.log({"loss": loss})
```

> 梯度、指标和图（Graph）不会被记录,直到`wandb.log`在forward和bacward通过后被调用。

请参阅[colab笔记本](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb) ，以了解将wandb与PyTorch集成的端到端示例，包括一个[视频教](https://www.youtube.com/watch?v=G7GH0SeNBMA&ab_channel=Weights&Biases)程 。你也可以在我们的[示例项目](https://docs.wandb.ai/v/zh-hans/examples)部分找到更多示例。

### **选项**

默认，钩子只记录梯度。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;</th>
      <th style="text-align:left">&#x9009;&#x9879;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>all: &#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x53C2;&#x6570;&#x76F4;&#x65B9;&#x56FE;</li>
          <li>gradients (&#x9ED8;&#x8BA4;)</li>
          <li>parameters (&#x6A21;&#x578B;&#x7684;&#x6743;&#x91CD;)</li>
          <li>None</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">&#x6574;&#x6570;(&#x9ED8;&#x8BA4;1000): &#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x4E4B;&#x95F4;&#x7684;&#x6B65;&#xFF08;step&#xFF09;&#x6570;</td>
    </tr>
  </tbody>
</table>

## **图像**

你可将带有图像数据的PyTorch tensors传入 `wandb.Image` 和torchvision utils，将用于自动记录它们。

要记录图像并在媒体（Media）面板中查看它们，你可以使用如下语法：

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## **多个模型**

如果你需要在同一个脚本中跟踪多个模型，你可以分别在每个模型上调用wandb.watch\(\) 。

## **示例**

我们已经为你创建了一些示例，以了解集成的工作原理：

* [在Google Colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)中运行: 一个简单的笔记本示例让你入门
* [Github](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py)上的示例: 一个Python脚本中的MNIST示例
* [Wandb 仪](https://app.wandb.ai/wandb/pytorch-mnist/runs/)表盘: 在W&B上查看结果

