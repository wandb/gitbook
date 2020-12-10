# Numpy Example

다음은 퍼셉트론\(perceptron\)을 훈련하고 결과를 W&B에 기록하는 raw numpy 코드의 전체 예시입니다.

 [GitHub](https://github.com/wandb/examples/blob/master/examples/machine-learning/numpy-boston/train.py)에서 코드를 확인하실 수 있습니다.

```python
from sklearn.datasets import load_boston
import numpy as np
import wandb

wandb.init()

# Save hyperparameters
wandb.config.lr = 0.000001
wandb.config.epochs = 1

# Load Dataset
data, target = load_boston(return_X_y=True)

# Initialize model
weights = np.zeros(data.shape[1])
bias = 0

# Train Model
for _ in range(wandb.config.epochs):
    np.random.shuffle(data)
    for i in range(data.shape[0]):
        x = data[i, :]
        y = target[i]

        err = y - np.dot(weights, x)
        if (err < 0):
            weights -= wandb.config.lr * x 
            bias -= wandb.config.lr
        else:
            weights += wandb.config.lr * x
            bias += wandb.config.lr

        # Log absolute error as "loss"
        wandb.log({"Loss": np.abs(err)})

# Save Model
np.save("weights", weights)
wandb.save("weights.npy")
```

