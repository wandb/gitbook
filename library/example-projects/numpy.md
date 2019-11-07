# Numpy Example

This is a complete example of raw numpy code that trains a perceptron and logs the results to W&B.

You can find the code on [GitHub](https://github.com/wandb/examples/blob/master/numpy-boston/train.py).

```python
from sklearn.datasets import load_bostonimport numpy as npimport wandbwandb.init()# Save hyperparameterswandb.config.lr = 0.000001wandb.config.epochs = 1# Load Datasetdata, target = load_boston(return_X_y=True)# Initialize modelweights = np.zeros(data.shape[1])bias = 0# Train Modelfor _ in range(wandb.config.epochs):    np.random.shuffle(data)    for i in range(data.shape[0]):        x = data[i, :]        y = target[i]        err = y - np.dot(weights, x)        if (err < 0):            weights -= wandb.config.lr * x             bias -= wandb.config.lr        else:            weights += wandb.config.lr * x            bias += wandb.config.lr        # Log absolute error as "loss"        wandb.log({"Loss": np.abs(err)})# Save Modelnp.save("weights", weights)wandb.save("weights.npy")
```

