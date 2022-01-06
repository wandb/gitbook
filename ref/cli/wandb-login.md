# wandb login

**Usage**

`wandb login [OPTIONS] [KEY]...`

**Summary**

Login to Weights & Biases

**Options**

| **Option**    | **Description**                                                  |
| ------------- | ---------------------------------------------------------------- |
| --cloud       | Login to the cloud instead of local                              |
| --host        | Login to a specific instance of W\&B                             |
| --relogin     | Force relogin if already logged in. The API key is re-requested. |
| --anonymously | Log in anonymously                                               |
| --help        | Show this message and exit.                                      |

#### Examples:

### Logging in to your own server

If you're hosting your own W\&B server, you'll need to specify the host parameter. You can do so with:

```shell
wandb login --host=https://YOURSERVER.COM
```

Or by setting the `WANDB_BASE_URL` environment variable which serves as an equivalent for host. For example:&#x20;

```shell
!WANDB_BASE_URL="https://YOURSERVER.COM" wandb login
```

### Relogin to public cloud

You can relogin back to public cloud instead of local with:

```shell
wandb login --cloud
```
