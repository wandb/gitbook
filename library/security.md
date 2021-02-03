# Security

Por simplicidad W&B usa las claves de la API para la autorización cuando se accede a la API. Puedes encontrar tus claves de la API en tus [ajustes](https://app.wandb.ai/settings). Tu clave de la API debería ser almacenada de forma segura y nunca debería estar bajo el control de versiones. En adición a las claves de la API personales, puedes agregar usuarios de la Cuenta de Servicios a tu equipo.

## Rotación de Claves

Tanto las claves personales como las de la cuenta de servicios pueden ser rotadas o revocadas. Simplemente crea una nueva Clave de la API, o un usuario de la Cuenta de Servicios, y reconfigura a tus scripts para que usen la nueva clave. Una vez que todos los procesos sean reconfigurados, puedes eliminar la clave de la API vieja de tu perfil o de tu equipo.

## Intercambio entre cuentas

Si tienes dos cuentas W&B funcionando desde la misma máquina, necesitarás una buena forma para intercambiar entre tus diferentes claves de la API. Puedes almacenar ambas claves de la API en un archivo en tu máquina y entonces agregar código como el siguiente a tus repositorios. Esto es para evitar incluir tu clave secreta en el sistema de control del código fuente, lo cual es potencialmente peligroso.

```text
if os.path.exists("~/keys.json"):
   os.environ["WANDB_API_KEY"] = json.loads("~/keys.json")["work_account"]
```

