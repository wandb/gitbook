# Catalyst

 Sergey Kolesnikov, créateur de [Catalyst](https://github.com/catalyst-team/catalyst), a construit une incroyable intégration W&B. Si vous utilisez Catalyst, nous avons un runner qui enregistre automatiquement tous les hyperparamètres, les mesures, les TensorBoard, le modèle le mieux entraîné, et tous les `stdout` pendant l’entraînement. 

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

 Des paramètres personnalisés peuvent aussi être donnés à cette étape. Les passes avant-arrière ainsi que la prise en charge des lots de données peuvent être personnalisées en étendant la classe runner. Ci-dessous, un `runner` personnalisé utilisé pour entraîner un classifieur MNIST.

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

 `logging_params` : tout paramètre ou fonction `wandb.init` à l’exception de `reinit` qui est automatiquement réglé sur True et de `dir` qui est réglé sur `<logdir>`  


```python
runner.train(...,
             ...,
             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],
             ...)
```

