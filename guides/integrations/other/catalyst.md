# Catalyst

 [Catalyst](https://github.com/catalyst-team/catalyst) is a PyTorch framework for Deep Learning R&D that focuses on reproducibility, rapid experimentation, and codebase reuse so you can create something new. Catalyst has an awesome W&B integration for logging parameters, metrics, images, and other artifacts.

You can set the following parameters in the `WandbLogger`

| Parameter | Description |
| :--- | :--- |
|  **project**  | Name of the project in W&B to log to. |
|  **name**  | Name of the run in W&B to log to |
|  **config**  | Configuration Dictionary for the experiment |
|  **entity**  | Name of W&B entity\(team\) to log to |

## Python API Examples

### SupervisedRunner

```python
from catalyst import dl

runner = dl.SupervisedRunner()
runner.train(
    ...,
    loggers={
    'wandb': dl.WandbLogger(
         project='wandb_catalyst',
         name='catalyst_supervised'
             )
        })

```

### CustomRunner

```python
from catalyst import dl

class CustomRunner(dl.IRunner):
    # ...

    def get_loggers(self):
        return {
            "console": dl.ConsoleLogger(),
            "wandb": dl.WandbLogger(
                    project="wandb_catalyst",
                    name="catalyst_runner"
                    )
        }

runner = CustomRunner().run()
```

### Config API

```python
loggers:
    wandb:
        _target_: WandbLogger
        project: test_exp
        name: test_run
...
```

### Hydra API

```python
loggers:
    wandb:
        _target_: catalyst.dl.WandbLogger
        project: test_exp
        name: test_run
...
```

## Interactive Example

Run this [example colab](https://colab.research.google.com/drive/1PD0LnXiADCtt4mu7bzv7VfQkFXVrPxJq?usp=sharing) to see Catalyst and W&B integration in action

