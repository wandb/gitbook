---
description: >-
  Registra y visualiza las ejecuciones sin crear una cuenta, y entrega el código
  a los revisores de publicaciones para que puedan correrlo y visualizarlo sin
  tener que configurar ellos mismos a Weights
---

# Anonymous Mode

 ¿Estás escribiendo una publicación para presentarla en una conferencia? Utiliza el modo anónimo para permitir que cualquier persona corra tu código y obtenga un tablero de control de Weights and Biases sin crear una cuenta.

```python
wandb.init(anonymous="allow")
```

### Uso de ejemplo

 [Prueba la notebook de ejemplo](http://bit.ly/anon-mode) para ver cómo funciona el modo anónimo en concreto.

```python
import wandb
import random

wandb.init(project="anon-demo", 
           anonymous="allow",
           config={
               "learning_rate": 0.1,
               "batch_size": 128,
           })

for step in range(30):
  wandb.log({
      "acc": random.random(),
      "loss": random.random()
  })
```

