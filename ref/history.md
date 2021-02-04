# History

## wandb.sdk.wandb\_history

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L3)

History hace el seguimiento de los datos registrados a través del tiempo. Para usar history desde tu script, llama a wandb.log\({"key": value}\) en un paso simple, o múltiples veces, desde tu ciclo de entrenamiento. Esto genera una serie de tiempos de escalares o medios guardados al historial.

En la Interfaz de Usuario, si registras un escalar con múltiples marcas de tiempo, por defecto W&B representará estas métricas del historial como un gráfico de líneas. Si registras un valor simple en el historial, lo comparará a través de las ejecuciones con un gráfico de barras.

A menudo, es útil hacer el seguimiento de una serie de tiempos completa, así también como de un valor de síntesis simple. Por ejemplo, la precisión en cada paso de History y la mejor precisión en Summary. Por defecto, Summary es establecido con el valor final de History.

### Objetos History

```python
class History(object)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L23)

Los datos de la serie de tiempos para los Runs. Esencialmente, esto es una lista de diccionarios en donde cada diccionario es un conjunto de estadísticas de síntesis registradas.

