# Login

## wandb.sdk.wandb\_login

[\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L3)

Connectez-vous à Weights & Biases, ce qui authentifie votre machine pour enregistrer des données sur votre compte.

**login**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L22)

Se connecte à W&B.

**Arguments**:

* `anonymous` _string, optionnel_ – Peut être "must", "allow", ou "never". Si réglé sur "must", nous enregistrerons toujours anonymement, si réglé sur "allow", nous ne créerons un utilisateur anonyme que si l’utilisateur courant n’est pas déjà connecté.
* `key` _string, optionnel_ – clef d’authentification
* `relogin` _bool, optionnel_ – Si True, demandera à nouveau la clef API.
* `host` _string, optionnel_ – L’hôte auquel se connecter.

 **Renvoie :**

* `bool` - si key est configuré

**Soulève :**

UsageError – si api\_key ne peut être configuré et sans tty

