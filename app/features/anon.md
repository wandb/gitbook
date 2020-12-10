---
description: >-
  계정 생성 없이 실행을 로깅 및 시각화하고, Weights & Biases 설정 없이 그 자체로 논문 검토자(paper
  reviewers)들이 실행하고 시각화 할 수 있는 코드를 제공합니다.
---

# Anonymous Mode

 컨퍼런스에 제출한 논문을 작성중이신가요? **익명 모드\(anonymous mode\)**를 사용하시면 계성 생성 없이 누구든지 여러분의 코드를 실행하고, Weights & Biases 대시보드를 얻을 수 있습니다.

```python
wandb.init(anonymous="allow")
```

##  **사용 예시**

 [해당 예시 notebook](http://bit.ly/anon-mode)을 사용해서 익명 모드가 어떻게 작동하는지 확인해보십시오.

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

