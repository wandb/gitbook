---
description: W&B integration with Hugging Face - Transformers
---

# Hugging Face - Transformers

[Hugging Face - Transformers](https://huggingface.co/transformers/) provides general-purpose architectures for Natural Language Understanding (NLU) and Natural Language Generation (NLG) with pretrained models in 100+ languages and deep interoperability between TensorFlow 2.0 and PyTorch.

Training is automatically logged on Weights & Biases when `wandb` is installed and user is logged in.

```
pip install transformers

pip install wandb
wandb login
```

The `Trainer` will automatically log losses, evaluation metrics, model topology & gradients.

Logging can be customized by overriding following environment parameters.

**Parameters**

* **WANDB_WATCH** (_Optional: ["gradients", "all", "false"]_) - "gradients" by default, set to "false" to disable gradient logging or "all" to log gradients and parameters
* **WANDB_PROJECT** (_Optional: str_) - "huggingface" by default, set this to a custom string to store results in a different project
* **WANDB_DISABLED** (_Optional: bool_) - defaults to false, set to "true" to disable wandb entirely

Experiment with our demo notebook and share your results with us!
