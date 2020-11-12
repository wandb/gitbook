# Pytorch

 权阈为PyTorch提供了一流的支持。为了自动记录梯度并存储网络拓扑，你可以调用watch并传入自己的PyTorch模型。

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

> . 先是一个正向传播加一个反向传播，之后调用`wandb.log`，直到这时才会记录梯度、指标和图形。

 请查看[Colab笔记本](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)，其中有一个wandb与PyTorch集成的端到端示例。另外，[范例项目](https://app.gitbook.com/@weights-and-biases/s/docs/examples)这一节包含了更多范例，大家可前往查看。

### **选项**

 默认情况下，钩子只记录梯度。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5B9E;&#x53C2;</th>
      <th style="text-align:left">&#x9009;&#x9879;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>&#x5168;&#x90E8;&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x548C;&#x5F62;&#x53C2;</li>
          <li>&#x68AF;&#x5EA6;&#xFF08;&#x9ED8;&#x8BA4;&#xFF09;</li>
          <li>&#x5F62;&#x53C2;&#xFF08;&#x6A21;&#x578B;&#x7684;&#x6743;&#x503C;&#xFF09;</li>
          <li>&#x7A7A;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">&#x6574;&#x6570;&#xFF08;&#x9ED8;&#x8BA4;&#x4E3A;100&#xFF09;&#xFF1A;&#x8BB0;&#x5F55;&#x68AF;&#x5EA6;&#x4E4B;&#x95F4;&#x7684;&#x6B65;&#x6570;&#x3002;</td>
    </tr>
  </tbody>
</table>

##  **图像**

 你可以把PyTorch张量和图像数据传入`wandb.Image`，将会用torchvision.utils自动记录它们。

 为了记录图像并在媒体面板中查看，你可以用以下语法：

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## Multiple Models **多个模型**

如果你需要在同一脚本中跟踪多个模型，就在每个模型都放上wandb.watch\(\)

## **范例**

 我们准备了几个例子，让大家看看集成的效果：

*  [在Colab运行](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)：从一个简单的笔记本范例入手；
*  [Github范例](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py)：MNIST范例；
* [Wa](https://app.wandb.ai/wandb/pytorch-mnist/runs/) [wandb指示板](https://wandb.ai/wandb/pytorch-mnist/runs/)：在权阈中查看结果。

