# Catalyst

Sergey Kolesnikov, [Catalyst](https://github.com/catalyst-team/catalyst)的创建者，已经构建了一个很棒的W&B集成。如果你正在使用Catalyst, 我们有一个Rrunner,可以自动记录所有超参数、指标、TensorBoard、最佳训练模型以及训练过程中的所有`stdout` 。

```text
import torchfrom catalyst.dl.supervised import SupervisedRunnerfrom catalyst.contrib.dl.callbacks import WandbLogger# experiment setuplogdir = "./logdir"num_epochs = 42​# dataloaders = {"train": ..., "valid": ...}​# model, criterion, optimizermodel = Net()criterion = torch.nn.CrossEntropyLoss()optimizer = torch.optim.Adam(model.parameters())scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer)​# model runnerrunner = SupervisedRunner()​# model trainingrunner.train(    model=model,    criterion=criterion,    optimizer=optimizer,    scheduler=scheduler,    loaders=loaders,    callbacks=[WandbLogger(project="Project Name",name= 'Run Name')],    logdir=logdir,    num_epochs=num_epochs,    verbose=True)
```

 自定义参数也可以在该阶段给出。还可以通过扩展 `runner` 类来定制与数据批处理相关的向前向后传递。以下是一个用于训练MNIST 分类器的自定义Runner。

```text
from catalyst import dlfrom catalyst.utils import metricsmodel = torch.nn.Linear(28*28, 10)​class CustomRunner(dl.Runner):    def _handle_batch(self, batch):        x, y = batch        y_hat = self.model(x.view(x.size(0), -1))        loss = F.cross_entropy(y_hat, y)        accuracy = metrics.accuracy(y_hat, y)​        #Set custom metric to be logged        self.batch_metrics = {            "loss": loss,            "accuracy": accuracy[0],​        }​        if self.is_train_loader:            loss.backward()            self.optimizer.step()            self.optimizer.zero_grad()runner = CustomRunner()     ​runner.train(    model=model,    criterion=criterion,    optimizer=optimizer,    scheduler=scheduler,    loaders=loaders,    num_epochs=num_epochs,    callbacks=[WandbLogger(project="catalyst",name= 'Example')],    verbose=True,    timeit=False)
```

## **选项** <a id="options"></a>

`logging_params`: 函数 `wandb.init` 的任何参数，除了`reinit` 会自动设置为`True` 和`dir` 设置为`<logdir>`

```text
runner.train(...,             ...,             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],             ...)
```

[  
](https://docs.wandb.ai/integrations/ray-tune)

