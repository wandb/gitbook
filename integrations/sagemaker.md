# SageMaker

## Integración de SageMaker

 W&B se integra con [Amazon SageMaker](https://aws.amazon.com/sagemaker/), leyendo automáticamente los hiperparámetros, agrupando las ejecuciones distribuidas, y reanudando ejecuciones a partir de los puntos de control.

### Autenticación

W&B busca un archivo llamado `secrets.env`, relativo al script de entrenamiento, y lo carga en el entorno cuando es llamado `wandb.init()`. Puedes generar un archivo `secrets.env` al llamar a wandb.sagemaker\_auth\(path="source\_dir"\) en el script que usas para lanzar tus experimentos. ¡Asegúrate de agregar este archivo a tu `.gitignore`!

###  Estimadores Existentes

 Si estás usando uno de los estimadores preconfigurados de SageMaker, necesitas agregar un `requirements.txt` a tu directorio fuente, que incluya a wandb

```text
wandb
```

Si estás usando un estimador que esté corriendo Python 2, necesitarás instalar psutil directamente desde un [wheel](https://pythonwheels.com/) antes de instalar wandb:

```text
https://wheels.galaxyproject.org/packages/psutil-5.4.8-cp27-cp27mu-manylinux1_x86_64.whl
wandb
```

  
Hay un ejemplo completo disponible en [GitHub](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cifar10-sagemaker), y puedes leer más en nuestro [blog](https://www.wandb.com/blog/running-sweeps-with-sagemaker).  


