# MMDetection

[MMDetection](https://github.com/open-mmlab/mmdetection/) is an open source object detection toolbox based on PyTorch and is part of the [OpenMMLab](https://openmmlab.com/) project.

## Getting Started

You can get started with Weights and Biases by adding the following hook to your  MMDetection code

```python
import wandb
...

log_config = dict(
            interval=10,
            hooks=[
                dict(type='WandbLogger',
                     wandb_init_kwargs={
                         'entity': WANDB_ENTITY,
                         'project': WANDB_PROJECT_NAME
                     },
                     logging_interval=10,
                     log_checkpoint=True,
                     log_checkpoint_metadata=True,
                     num_eval_images=100)
            ])
```

## Example

Any questions or issues about this Weights & Biases integration? Open an issue in the [MMDetection github repository](https://github.com/open-mmlab/mmdetection) and we'll catch it and get you an answer :)
