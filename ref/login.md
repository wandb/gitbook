# Login

## wandb.sdk.wandb\_login

[\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L3)

Inicia sesión en Weights & Biases, autenticando a tu máquina para registrar datos a tu cuenta.

**login**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

 [\[ver fuente\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L22)

Inicia sesión en W&B.

 **Argumentos:**

* `anonymous` _string, optional_ - Puede ser “must”, “allow” o “never”.

  Si es establecido a “must” siempre hará los registros de forma anónima, si es establecido a “allow” solamente crearemos un usuario anónimo si el usuario aún no ha iniciado sesión.

* `key` _string, optional_ - clave de autenticación.
* `relogin` _bool, optional_ - si es true, volveremos a solicitar la clave de la API.
* `host` _string, optional_ - el host al que hay que conectarse.

**Devueve:**

* `bool` - si la clave está configurada

#### Levanta:

UsageError – si api\_key no puede ser configurada y no hay tty

