# wandb docker-run

**Usage**

`wandb docker-run [OPTIONS] [DOCKER_RUN_ARGS]...`

**Summary**

Simple wrapper for `docker run` which adds WANDB_API_KEY and WANDB_DOCKER
environment variables to any docker run command.

This will also set the runtime to nvidia if the nvidia-docker executable is
present on the system and --runtime wasn't set.

See `docker run --help` for more details.

**Options**

| **Option** | **Description** |
| :--- | :--- |
| --help | Show this message and exit. |

