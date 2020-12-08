---
description: >-
  Log and visualize runs without creating an account, and give paper reviewers
  code that they can run and visualize without setting up Weights & Biases
  themselves
---

# Anonymous Mode

Are you writing a paper to submit to a conference? Use **anonymous mode** to let anyone run your code and get a Weights & Biases dashboard without creating an account.

```python
wandb.init(anonymous="allow")
```

## Example usage

[Try the example notebook](http://bit.ly/anon-mode) to see how anonymous mode works in action.

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

