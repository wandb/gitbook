---
description: >-
  Inicia sesión, restituye el estado del código, sincroniza directorios locales
  con nuestros servidores, y corre barridos de hiperparámetros con nuestra
  interfaz de la línea de comandos
---

# Command Line Interface

 Después de correr `pip install wandb` deberías tener un nuevo comando disponible, **wandb**.

Están disponibles los siguientes subcomandos:

| Subcomando | Descripción |
| :--- | :--- |
| docs | Abre la documentación en un navegador |
| init | Configura un directorio con W&B |
| login | Inicia sesión en W&B |
| offline | Sólo guarda los datos de la ejecución localmente, no hay sincronización con la nube \(`off` es obsoleto\) |
| online | Se asegura de que W&B esté habilitado en este directorio \(`on` es obsoleto\) |
| disabled | Deshabilita todas las llamadas a la API, es útil para el testing |
| enabled |  Igual que online, continua con el registro regular de W&B una vez que hayas finalizado el testing |
| docker | Corre una imagen docker, monta cwd, y se asegura de que wandb esté instalado |
| docker-run | Agrega variable de entorno de W&B a un comando de ejecución de Docker |
| projects | Lista los proyectos |
| pull | Toma archivos para una ejecución desde W&B |
| restore | Restituye el código y configura el estado para una ejecución |
| run | Lanza un programa que no está escrito en python, para que python utilice wandb.init\(\) |
| runs | Lista las ejecuciones en un proyecto |
| sync | Sincroniza un directorio local conteniendo tfevents o archivos de ejecuciones previas |
| status | Lista el estado actual del directorio |
| sweep | Crea un nuevo barrido, dada una definición YAML |
| agent | Comienza un agente para correr programas en el barrido |

## Restituye el estado de tu código

Usa `restore` para volver al estado de tu código cuando corriste una ejecución dada.

### Ejemplo

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

**¿Cómo capturamos el estado del código?**

Cuando `wandb.init` es llamado desde tu script, es guardado un enlace al último git commit, si el código está en un repositorio git. También es creado un parche diff en el caso en que haya cambios no confirmados o cambios que no estén sincronizados con el servidor remoto.

