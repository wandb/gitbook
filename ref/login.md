# Login

## wandb.sdk.wandb\_login

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L3)

Log in to Weights & Biases, authenticating your machine to log data to your account.

**login**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

[\[view\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_login.py#L22)

Log in to W&B.

**Arguments**:

* `anonymous` _string, optional_ - Can be "must", "allow", or "never".

  If set to "must" we'll always login anonymously, if set to

  "allow" we'll only create an anonymous user if the user

  isn't already logged in.

* `key` _string, optional_ - authentication key.
* `relogin` _bool, optional_ - If true, will re-prompt for API key.
* `host` _string, optional_ - The host to connect to.

**Returns**:

* `bool` - if key is configured

**Raises**:

UsageError - if api\_key can not configured and no tty

