# Command Line Reference

## wandb

**Usage**

`wandb [OPTIONS] COMMAND [ARGS]...`

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --version |  | Show the version and exit. |
| --help |  | Show this message and exit. |

**Commands**

| **Commands** | **Type** | **Description** |
| :--- | :--- | :--- |
| agent |  | Run the W&B agent |
| artifact |  | Commands for interacting with artifacts |
| controller |  | Run the W&B local sweep controller |
| disabled |  | Disable W&B. |
| docker |  | docker lets you run your code in a docker image ensuring... |
| docker-run |  | Simple wrapper for `docker run` which sets W&B environment... |
| enabled |  | Enable W&B. |
| init |  | Configure a directory with Weights & Biases |
| local |  | Launch local W&B container \(Experimental\) |
| login |  | Login to Weights & Biases |
| offline |  | Disable W&B sync |
| online |  | Enable W&B sync |
| pull |  | Pull files from Weights & Biases |
| restore |  | Restore code, config and docker state for a run |
| status |  | Show configuration settings |
| sweep |  | Create a sweep |
| sync |  | Upload an offline training directory to W&B |

## wandb agent

**Usage**

`wandb agent [OPTIONS] SWEEP_ID`

**Summary**

Run the W&B agent

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -p, --project | TEXT | The project of the sweep. |
| -e, --entity | TEXT | The entity scope for the project. |
| --count | INTEGER | The max number of runs for this agent. |
| --help |  | Show this message and exit. |

## wandb artifact

**Usage**

`wandb artifact [OPTIONS] COMMAND [ARGS]...`

**Summary**

Commands for interacting with artifacts

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --help |  | Show this message and exit. |

### wandb artifact get

**Usage**

`wandb artifact get [OPTIONS] PATH`

**Summary**

Download an artifact from wandb

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --root | TEXT | The directory you want to download the artifact to |
| --type | TEXT | The type of artifact you are downloading |
| --help |  | Show this message and exit. |

### wandb artifact ls

**Usage**

`wandb artifact ls [OPTIONS] PATH`

**Summary**

List all artifacts in a wandb project

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -t, --type | TEXT | The type of artifacts to list |
| --help |  | Show this message and exit. |

### wandb artifact put

**Usage**

`wandb artifact put [OPTIONS] PATH`

**Summary**

Upload an artifact to wandb

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -n, --name | TEXT | The name of the artifact to push: |
| -d, --description | TEXT | A description of this artifact |
| -t, --type | TEXT | The type of the artifact |
| -a, --alias | TEXT | An alias to apply to this artifact |
| --help |  | Show this message and exit. |

## wandb controller

**Usage**

`wandb controller [OPTIONS] SWEEP_ID`

**Summary**

Run the W&B local sweep controller

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --verbose |  | Display verbose output |
| --help |  | Show this message and exit. |

## wandb disabled

**Usage**

`wandb disabled [OPTIONS]`

**Summary**

Disable W&B.

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --help |  | Show this message and exit. |

## wandb docker

**Usage**

`wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

**Summary**

W&B docker lets you run your code in a docker image ensuring wandb is configured. It adds the WANDB\_DOCKER and WANDB\_API\_KEY environment variables to your container and mounts the current directory in /app by default. You can pass additional args which will be added to `docker run` before the image name is declared, we'll choose a default image for you if one isn't passed:

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow- images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker wandb/deepo:keras-gpu --no-tty --cmd "python train.py --epochs=5"

By default we override the entrypoint to check for the existance of wandb and install it if not present. If you pass the --jupyter flag we will ensure jupyter is installed and start jupyter lab on port 8888. If we detect nvidia-docker on your system we will use the nvidia runtime. If you just want wandb to set environment variable to an existing docker run command, see the wandb docker-run command.

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --nvidia |  | / --no-nvidia    Use the nvidia runtime, defaults to nvidia if |
| nvidia-docker |  | is present |
| --digest |  | Output the image digest and exit |
| --jupyter |  | / --no-jupyter  Run jupyter lab in the container |
| --dir | TEXT | Which directory to mount the code in the container |
| --no-dir |  | Don't mount the current directory |
| --shell | TEXT | The shell to start the container with |
| --port | TEXT | The host port to bind jupyter on |
| --cmd | TEXT | The command to run in the container |
| --no-tty |  | Run the command without a tty |
| --help |  | Show this message and exit. |

## wandb enabled

**Usage**

`wandb enabled [OPTIONS]`

**Summary**

Enable W&B.

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --help |  | Show this message and exit. |

## wandb init

**Usage**

`wandb init [OPTIONS]`

**Summary**

Configure a directory with Weights & Biases

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -p, --project | TEXT | The project to use. |
| -e, --entity | TEXT | The entity to scope the project to. |
| --reset |  | Reset settings |
| -m, --mode | TEXT | Can be "online", "offline" or "disabled". Defaults to |
| --help |  | Show this message and exit. |

## wandb local

**Usage**

`wandb local [OPTIONS]`

**Summary**

Launch local W&B container \(Experimental\)

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -p, --port | TEXT | The host port to bind W&B local on |
| -e, --env | TEXT | Env vars to pass to wandb/local |
| --daemon |  | / --no-daemon  Run or don't run in daemon mode |
| --upgrade |  | Upgrade to the most recent version |
| --help |  | Show this message and exit. |

## wandb login

**Usage**

`wandb login [OPTIONS] [KEY]...`

**Summary**

Login to Weights & Biases

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --cloud |  | Login to the cloud instead of local |
| --host | TEXT | Login to a specific instance of W&B |
| --relogin |  | Force relogin if already logged in. |
| --anonymously |  | Log in anonymously |
| --help |  | Show this message and exit. |

## wandb offline

**Usage**

`wandb offline [OPTIONS]`

**Summary**

Disable W&B sync

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --help |  | Show this message and exit. |

## wandb online

**Usage**

`wandb online [OPTIONS]`

**Summary**

Enable W&B sync

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --help |  | Show this message and exit. |

## wandb pull

**Usage**

`wandb pull [OPTIONS] RUN`

**Summary**

Pull files from Weights & Biases

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -p, --project | TEXT | The project you want to download. |
| -e, --entity | TEXT | The entity to scope the listing to. |
| --help |  | Show this message and exit. |

## wandb restore

**Usage**

`wandb restore [OPTIONS] RUN`

**Summary**

Restore code, config and docker state for a run

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --no-git |  | Skupp |
| --branch |  | / --no-branch  Whether to create a branch or checkout detached |
| -p, --project | TEXT | The project you wish to upload to. |
| -e, --entity | TEXT | The entity to scope the listing to. |
| --help |  | Show this message and exit. |

## wandb status

**Usage**

`wandb status [OPTIONS]`

**Summary**

Show configuration settings

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --settings |  | / --no-settings  Show the current settings |
| --help |  | Show this message and exit. |

## wandb sweep

**Usage**

`wandb sweep [OPTIONS] CONFIG_YAML`

**Summary**

Create a sweep

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| -p, --project | TEXT | The project of the sweep. |
| -e, --entity | TEXT | The entity scope for the project. |
| --controller |  | Run local controller |
| --verbose |  | Display verbose output |
| --name | TEXT | Set sweep name |
| --program | TEXT | Set sweep program |
| --update | TEXT | Update pending sweep |
| --help |  | Show this message and exit. |

## wandb sync

**Usage**

`wandb sync [OPTIONS] [PATH]...`

**Summary**

Upload an offline training directory to W&B

**Options**

| **Options** | **Type** | **Description** |
| :--- | :--- | :--- |
| --id | TEXT | The run you want to upload to. |
| -p, --project | TEXT | The project you want to upload to. |
| -e, --entity | TEXT | The entity to scope to. |
| --include-globs | TEXT | Comma seperated list of globs to include. |
| --exclude-globs | TEXT | Comma seperated list of globs to exclude. |
| --include-online |  | / --no-include-online |
| Include |  | online runs |
| --include-offline |  | / --no-include-offline |
| Include |  | offline runs |
| --include-synced |  | / --no-include-synced |
| Include |  | synced runs |
| --mark-synced |  | / --no-mark-synced |
| Mark |  | runs as synced |
| --sync-all |  | Sync all runs |
| --clean |  | Delete synced runs |
| --clean-old-hours | INTEGER | Delete runs created before this many hours. |
| To |  | be used alongside --clean flag. |
| --clean-force |  | Clean without confirmation prompt. |
| --show | INTEGER | Number of runs to show |
| --help |  | Show this message and exit. |

