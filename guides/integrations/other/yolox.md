# YOLOX

YOLOX is an anchor-free version of YOLO with strong performance for object detection.



## Getting Started

To use YOLOX with Weighs & Biases just use the `--logger wandb` command line argument to turn on logging with wandb. You can also pass all of the&#x20;

```python
python tools/train.py .... --logger wandb \
                wandb-project <project-name> \
                wandb-entity <entity>
                wandb-name <run-name> \
                wandb-id <run-id> \
                wandb-save_dir <save-dir> \
                wandb-num_eval_imges <num-images> \
                wandb-log_checkpoints <bool>
```
