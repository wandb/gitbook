---
description: >-
  アカウントを作成せずに実行をログに記録して視覚化し、Weights＆Biases自体を設定せずに実行および視覚化できるコードを論文のレビュー担当者に提供します
---

# Anonymous Mode

会議に提出する論文を書いていますか？匿名モードを使用すると、アカウントを作成せずに、誰でもコードを実行して、Weights＆Biasesダッシュボードを取得できます。

```python
wandb.init(anonymous="allow")
```

## 使用例

[サンプルノートブックを試して](https://colab.research.google.com/drive/1nQ3n8GD6pO-ySdLlQXgbz4wA3yXoSI7i?usp=sharing)、匿名モードが実際にどのように機能するかを確認してください。

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

