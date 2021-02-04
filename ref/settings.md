# Settings

## wandb.sdk.wandb\_settings

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L2)

Este módulo configura los ajustes para las ejecuciones de wandb.

Orden de los ajustes de carga: \(difiere de la prioridad\), por defecto es environment wandb.setup\(settings=\) system\_config workspace\_config wandb.init\(settings=\) network\_org network\_entity network\_project.

Prioridad de los ajustes: Ver la variable “source”.

Cuando es usado override, tiene prioridad sobre los ajustes que no usen override.

Las prioridades de override están en un orden reverso respecto al de los ajustes que no usan override.

### Objetos Settings

```python
class Settings(object)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L187)

 Constructor de Settings

#### Argumentos:

* `entity` - usuario personal o equipo que hace uso de Run
* `project` - nombre del proyecto para Run

 **Levanta:**

* `Exception` - si hay un problema.

**\_\_copy\_\_**

```python
 | __copy__()
```

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L656)

Copia \(notar que el objeto copiado no será congelado\)

