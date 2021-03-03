# Catalyst

 [Catalyst](https://github.com/catalyst-team/catalyst) 개발자인 Sergey Kolesnikov는 놀라운 W&B 통합을 개발했습니다. Catalyst를 사용하시는 경우, 모든 초매개변수, 메트릭, TensorBoard, 최고로 훈련된 모델 및 모든 `stdout`을 훈련 중에 로그할 수 있는 러너\(runner\)가 있습니다.  


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

  사용자 정의 매개변수는 또한 해당 단계에서 주어질 수도 있습니다. 데이터 배치 처리와 함께 정방향\(forward\) 및 역방향\(backward\) 패스\(pass\) 또한 `runner` 클래스 확장을 통해 사용자 정의될 수 있습니다. 다음은 MNIST 분류기\(classifier\) 훈련을 위해 사용되는 사용자 정의 runner입니다.

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

##  **옵션**

`logging_params`:  `True`로 자동 설정되는 `reinit` 및 &lt;logdir&gt;로 설정된 `dir`을 제외한 모든 함수 `wandb.init`의 매개변수

```python
runner.train(...,
             ...,
             callbacks=[WandbLogger(project="catalyst",name= 'Example'),logging_params={params}],
             ...)
```

