# Catalyst

Sergey Kolesnikov, creator of [Catalyst](https://github.com/catalyst-team/catalyst), has built an awesome W&B integration. If you are using Catalyst, we have a runner that can automatically log all hyperparameters, metrics, TensorBoard, the best trained model, and all `stdout` during training.

```python
import torch
from catalyst.dl.supervised import SupervisedRunner
from catalyst.contrib.dl.callbacks import WandbLogger
# experiment setup
logdir = "./logdir"
num_epochs = 42

# data
loaders = {"train": ..., "valid": ...}

# model, criterion, optimizer
model = Net()
criterion = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters())
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer)

# model runner
runner = SupervisedRunner()

# model training
runner.train(
    model=model,
    criterion=criterion,
    optimizer=optimizer,
    scheduler=scheduler,
    loaders=loaders,
    callbacks=[WandbLogger(project="Project Name",name= 'Run Name')],
    logdir=logdir,
    num_epochs=num_epochs,
    verbose=True
)
```

Custom parameters can also be given at that stage. Forward and backward passes alsong with the handling of data batches can also be customized by extending the `runner` class. Following is a custom runner used to train a MNIST classifier.

```python
from catalyst import dl
from catalyst.utils import metrics
model = torch.nn.Linear(28*28, 10)

class CustomRunner(dl.Runner):
    def _handle_batch(self, batch):
        x, y = batch
        y_hat = self.model(x.view(x.size(0), -1))
        loss = F.cross_entropy(y_hat, y)
        accuracy = metrics.accuracy(y_hat, y)

        #Set custom metric to be logged
        self.batch_metrics = {
            "loss": loss,
            "accuracy": accuracy[0],

        }

        if self.is_train_loader:
            loss.backward()
            self.optimizer.step()
            self.optimizer.zero_grad()
runner = CustomRunner()     

runner.train(
    model=model,
    criterion=criterion,
    optimizer=optimizer,
    scheduler=scheduler,
    loaders=loaders,
    num_epochs=num_epochs,
    callbacks=[WandbLogger(project="catalyst",name= 'Example')],
    verbose=True,
    timeit=False)
```

## Options

`logging_params`: any parameters of function `wandb.init` except `reinit` which is automatically set to `True` and `dir` which is set to `<logdir>`

```python
runner.train(...,
             ...,
             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],
             ...)
```

