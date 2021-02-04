# Summary

## wandb.sdk.wandb\_summary

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L2)

### Objetos SummaryDict

```python
@six.add_metaclass(abc.ABCMeta)
class SummaryDict(object)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L18)

De tipo diccionario, engloba a todos los diccionarios anidados en un SummarySubDict y dispara a triggers self.\_root.\_callback cuando cambia la propiedad.

### Objetos Summary

```python
class Summary(SummaryDict)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L78)

Summary

Las estadísticas de síntesis son utilizadas para hacer el seguimiento de las métricas simples por cada modelo. Al llamar a wandb.log\({'accuracy': 0.9}\), wandb.summary\['accuracy'\] será establecida automáticamente a 0.9 a menos que wandb.summary\['accuracy'\] haya sido cambiado manualmente por el código.

Establecer manualmente a wandb.summary\['accuracy'\] puede ser útil si quieres mantener un registro de la precisión del mejor modelo, mientras utilizas wandb.log\(\) para llevar un registro de la precisión en cada paso.

Puedes querer almacenar las métricas de evaluación en una síntesis de las ejecuciones, después de que el entrenamiento se haya completado. La síntesis puede manejar arreglos numpy, tensores de pytorch o tensores de tensorflow. Cuando un valor corresponde a uno de estos tipos, persistimos el tensor entero en un archivo binario y almacenamos las métricas de alto nivel, tales como mínimo, media, varianza , percentil 95, et., en el objeto summary.

**Ejemplos:**

```text
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
wandb.run.summary["best_accuracy"] = test_accuracy
best_accuracy = test_accuracy
```

###  Objetos SummarySubDict

```python
class SummarySubDict(SummaryDict)
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L128)

Nodo, que no es el nodo raíz, de la estructura de datos summary.

