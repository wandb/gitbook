# Catalyst

Sergey Kolesnikov, creador de [Catalyst](https://github.com/catalyst-team/catalyst), ha construido una maravillosa integración con W&B. Si estás usando Catalyst, tenemos un runner que puede registrar automáticamente todos los hiperparámetros, las métricas, TensorBoard, al mejor modelo entrenado, y a todas las salidas a stdout durante el entrenamiento.

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

En esta etapa, también pueden ser dados los parámetros personalizado. Las pasadas hacia adelante y hacia atrás, conjuntamente con el manejo de los lotes de datos, también pueden ser personalizados al extender la clase runner. El siguiente es un runner personalizado utilizado para entrenar a un clasificador MNIST.

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

## Opciones

`logging_params`: cualquier parámetro de la función `wandb.init`, excepto por reinit que es automáticamente establecido a True y dir que es establecido a 

```python
runner.train(...,
             ...,
             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],
             ...)
```

