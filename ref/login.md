# Login Reference

## wandb.sdk.wandb\_login

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L3)

Log in to Weights & Biases, authenticating your machine to log data to your account.

**login**

```python
login(anonymous=None, key=None, relogin=None, host=None, force=None)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L22)

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

### \_WandbLogin Objects

```python
class _WandbLogin(object)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L45)

**\_\_init\_\_**

```python
 | __init__()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L46)

**setup**

```python
 | setup(kwargs)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L54)

**is\_apikey\_configured**

```python
 | is_apikey_configured()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L69)

**set\_backend**

```python
 | set_backend(backend)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L72)

**set\_silent**

```python
 | set_silent(silent)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L75)

**login**

```python
 | login()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L78)

**login\_display**

```python
 | login_display()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L90)

**configure\_api\_key**

```python
 | configure_api_key(key)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L110)

**update\_session**

```python
 | update_session(key)
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L124)

**prompt\_api\_key**

```python
 | prompt_api_key()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L135)

**propogate\_login**

```python
 | propogate_login()
```

[\[view\_source\]](https://github.com/wandb/client/blob/bf98510754bad9e6e2b3e857f123852841a4e7ed/wandb/sdk/wandb_login.py#L148)

