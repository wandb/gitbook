# Catalyst

 [Catalyst](https://github.com/catalyst-team/catalyst)の作成者であるSergey Kolesnikov氏は、すばらしいW＆B統合を構築しました。あなたがCatalystを使用している場合は、すべてのハイパーパラメータ、メトリック、TensorBoard、最適なトレーニング済みモデル、およびトレーニング中のすべての`stdout`を自動的にログに記録できるアプリがあります。

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

 その段階でカスタムパラメータを指定することもできます。データバッチの処理を伴うフォワードパスとバックワードパスも、`runner`クラスを拡張することでカスタマイズできます。以下は、MNIST分類器のトレーニングに使用されるカスタムランナーです

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

## オプション

`logging_params`：自動的にTrueに設定される`reinit`と&lt;logdir&gt;に設定されるdirを除く、関数`wandb.init`のすべてのパラメータ。

```python
runner.train(...,
             ...,
             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],
             ...)
```

