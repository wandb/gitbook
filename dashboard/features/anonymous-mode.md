---
description: 无需创建账户即可记录和可视化运行，并给论文审稿人提供他们无需自己设置Weights & Biases即可运行和可视化的代码。
---

# Anonymous Mode

你是否正在为一个会议准备一片论文？使用**匿名模式**，让任何人可以运行你的代码，并且无需创建账户即可获得Weights & Biases仪表盘。

```python
wandb.init(anonymous="allow")
```

## **使用示例**

 [试试这个示例笔记本](https://colab.research.google.com/drive/1nQ3n8GD6pO-ySdLlQXgbz4wA3yXoSI7i?usp=sharing)，看下匿名模式是如何运作的。

```python
import wandb
import random

wandb.init(project="anon-demo", 
           anonymous="allow",
           config={
               "learning_rate": 0.1,
               "batch_size": 128,
           })

for step in range(30):
  wandb.log({
      "acc": random.random(),
      "loss": random.random()
  })
```

