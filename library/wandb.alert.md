---
description: >-
  Alertas programables lanzadas desde Python, que se te envían a través de Slack
  o de la casilla de correos.
---

# wandb.alert\(\)

Envía una alerta a Slack o a tu casilla de correos, disparada desde tu script de Python.

1.   [Establece alertas en tu cuenta →](https://docs.wandb.ai/app/features/alerts)
2.  [Prueba el código →](http://tiny.cc/wb-alerts)
3. Verifica tu Slack o tu casilla de correos para ver las alertas programables.

###  Argumentos

`wandb.alert(title="Low Acc", text="Accuracy is below the expected threshold")`

* **title \(string\)**: Una descripción corta de la alerta, por ejemplo “Baja precisión”
* **text \(string\)**: Una descripción más larga y más detallada de lo que ocurrió para disparar la alerta
* **level \(optional\):** Describe la importancia de la alerta – debe ser `INFO`, `WARN` o `ERROR`
* **wait\_duration \(optional\):** Cuántos segundos hay que esperar antes de enviar otra alerta con el mismo title. Esto ayuda a reducir el spam de alertas.

###  Ejemplo

 Esta alerta simple envía una advertencia cuando la precisión cae por debajo de un umbral. Para evitar el spam, solamente envía alertas con un intervalo de al menos 5 minutos.

 [Ejecuta](http://tiny.cc/wb-alerts)[ el código→](http://tiny.cc/wb-alerts)

```python
from datetime import timedelta
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title='Low accuracy', 
        text=f'Accuracy {acc} is below the acceptable theshold {threshold}',
        level=AlertLevel.WARN,
        wait_duration=timedelta(minutes=5)
    )
```

