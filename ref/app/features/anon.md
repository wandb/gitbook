---
description: Log and visualize data without a W&B account
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

