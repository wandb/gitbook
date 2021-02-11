# Login

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_login.py#L22-L43)

Log in to W&B.

```text
login(
    anonymous=None, key=None, relogin=None, host=None, force=None
)
```

| Arguments |  |
| :--- | :--- |
|  `anonymous` |  \(string, optional\) Can be "must", "allow", or "never". If set to "must" we'll always login anonymously, if set to "allow" we'll only create an anonymous user if the user isn't already logged in. |
|  `key` |  \(string, optional\) authentication key. |
|  `relogin` |  \(bool, optional\) If true, will re-prompt for API key. |
|  `host` |  \(string, optional\) The host to connect to. |

| Returns |  |
| :--- | :--- |
|  `bool` |  if key is configured |

| Raises |
| :--- |
|  UsageError - if api\_key can not configured and no tty |

