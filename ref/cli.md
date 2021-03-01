# wandb

**Usage**

` wandb [OPTIONS] COMMAND [ARGS]...`



**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--version|Show the version and exit.|
|--help|Show this message and exit.|


**Commands**
| **Commands** | **Description** |
|:--|:--|:--|
|agent|Run the W&B agent|
|artifact|Commands for interacting with artifacts|
|controller|Run the W&B local sweep controller|
|disabled|Disable W&B.|
|docker|docker lets you run your code in a docker image ensuring...|
|docker-run|Simple wrapper for `docker run` which sets W&B environment...|
|enabled|Enable W&B.|
|init|Configure a directory with Weights & Biases|
|local|Launch local W&B container (Experimental)|
|login|Login to Weights & Biases|
|offline|Disable W&B sync|
|online|Enable W&B sync|
|pull|Pull files from Weights & Biases|
|restore|Restore code, config and docker state for a run|
|status|Show configuration settings|
|sweep|Create a sweep|
|sync|Upload an offline training directory to W&B|
|verify|Verify your local instance|
# wandb agent

**Usage**

` wandb agent [OPTIONS] SWEEP_ID`

**Summary**

Run the W&B agent


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-p, --project|The project of the sweep.|
|-e, --entity|The entity scope for the project.|
|--count|The max number of runs for this agent.|
|--help|Show this message and exit.|


# wandb artifact

**Usage**

` wandb artifact [OPTIONS] COMMAND [ARGS]...`

**Summary**

Commands for interacting with artifacts


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


## wandb artifact cache

**Usage**

` wandb artifact cache [OPTIONS] COMMAND [ARGS]...`

**Summary**

Commands for interacting with the artifact cache


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


## wandb artifact cache cleanup

**Usage**

` wandb artifact cache cleanup [OPTIONS] TARGET_SIZE`

**Summary**

Clean up less frequently used files from the artifacts cache


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


## wandb artifact get

**Usage**

` wandb artifact get [OPTIONS] PATH`

**Summary**

Download an artifact from wandb


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--root|The directory you want to download the artifact to|
|--type|The type of artifact you are downloading|
|--help|Show this message and exit.|


## wandb artifact ls

**Usage**

` wandb artifact ls [OPTIONS] PATH`

**Summary**

List all artifacts in a wandb project


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-t, --type|The type of artifacts to list|
|--help|Show this message and exit.|


## wandb artifact put

**Usage**

` wandb artifact put [OPTIONS] PATH`

**Summary**

Upload an artifact to wandb


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-n, --name|The name of the artifact to push:|
|-d, --description|A description of this artifact|
|-t, --type|The type of the artifact|
|-a, --alias|An alias to apply to this artifact|
|--help|Show this message and exit.|


# wandb controller

**Usage**

` wandb controller [OPTIONS] SWEEP_ID`

**Summary**

Run the W&B local sweep controller


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--verbose|Display verbose output|
|--help|Show this message and exit.|


# wandb disabled

**Usage**

` wandb disabled [OPTIONS]`

**Summary**

Disable W&B.


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


# wandb docker

**Usage**

` wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

**Summary**

W&B docker lets you run your code in a docker image ensuring wandb is
configured. It adds the WANDB_DOCKER and WANDB_API_KEY environment
variables to your container and mounts the current directory in /app by
default.  You can pass additional args which will be added to `docker run`
before the image name is declared, we'll choose a default image for you if
one isn't passed:

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow-
images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker
wandb/deepo:keras-gpu --no-tty --cmd "python train.py --epochs=5"

By default we override the entrypoint to check for the existance of wandb
and install it if not present.  If you pass the --jupyter flag we will
ensure jupyter is installed and start jupyter lab on port 8888.  If we
detect nvidia-docker on your system we will use the nvidia runtime.  If
you just want wandb to set environment variable to an existing docker run
command, see the wandb docker-run command.


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--nvidia|/ --no-nvidia    Use the nvidia runtime, defaults to nvidia if|
|nvidia-docker|is present|
|--digest|Output the image digest and exit|
|--jupyter|/ --no-jupyter  Run jupyter lab in the container|
|--dir|Which directory to mount the code in the container|
|--no-dir|Don't mount the current directory|
|--shell|The shell to start the container with|
|--port|The host port to bind jupyter on|
|--cmd|The command to run in the container|
|--no-tty|Run the command without a tty|
|--help|Show this message and exit.|


# wandb enabled

**Usage**

` wandb enabled [OPTIONS]`

**Summary**

Enable W&B.


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


# wandb init

**Usage**

` wandb init [OPTIONS]`

**Summary**

Configure a directory with Weights & Biases


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-p, --project|The project to use.|
|-e, --entity|The entity to scope the project to.|
|--reset|Reset settings|
|-m, --mode|Can be "online", "offline" or "disabled". Defaults to|
|--help|Show this message and exit.|


# wandb local

**Usage**

` wandb local [OPTIONS]`

**Summary**

Launch local W&B container (Experimental)


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-p, --port|The host port to bind W&B local on|
|-e, --env|Env vars to pass to wandb/local|
|--daemon|/ --no-daemon  Run or don't run in daemon mode|
|--upgrade|Upgrade to the most recent version|
|--help|Show this message and exit.|


# wandb login

**Usage**

` wandb login [OPTIONS] [KEY]...`

**Summary**

Login to Weights & Biases


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--cloud|Login to the cloud instead of local|
|--host|Login to a specific instance of W&B|
|--relogin|Force relogin if already logged in.|
|--anonymously|Log in anonymously|
|--help|Show this message and exit.|


# wandb offline

**Usage**

` wandb offline [OPTIONS]`

**Summary**

Disable W&B sync


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


# wandb online

**Usage**

` wandb online [OPTIONS]`

**Summary**

Enable W&B sync


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--help|Show this message and exit.|


# wandb pull

**Usage**

` wandb pull [OPTIONS] RUN`

**Summary**

Pull files from Weights & Biases


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-p, --project|The project you want to download.|
|-e, --entity|The entity to scope the listing to.|
|--help|Show this message and exit.|


# wandb restore

**Usage**

` wandb restore [OPTIONS] RUN`

**Summary**

Restore code, config and docker state for a run


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--no-git|Skupp|
|--branch|/ --no-branch  Whether to create a branch or checkout detached|
|-p, --project|The project you wish to upload to.|
|-e, --entity|The entity to scope the listing to.|
|--help|Show this message and exit.|


# wandb status

**Usage**

` wandb status [OPTIONS]`

**Summary**

Show configuration settings


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--settings|/ --no-settings  Show the current settings|
|--help|Show this message and exit.|


# wandb sweep

**Usage**

` wandb sweep [OPTIONS] CONFIG_YAML`

**Summary**

Create a sweep


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|-p, --project|The project of the sweep.|
|-e, --entity|The entity scope for the project.|
|--controller|Run local controller|
|--verbose|Display verbose output|
|--name|Set sweep name|
|--program|Set sweep program|
|--update|Update pending sweep|
|--help|Show this message and exit.|


# wandb sync

**Usage**

` wandb sync [OPTIONS] [PATH]...`

**Summary**

Upload an offline training directory to W&B


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--id|The run you want to upload to.|
|-p, --project|The project you want to upload to.|
|-e, --entity|The entity to scope to.|
|--include-globs|Comma seperated list of globs to include.|
|--exclude-globs|Comma seperated list of globs to exclude.|
|--include-online|/ --no-include-online|
|Include|online runs|
|--include-offline|/ --no-include-offline|
|Include|offline runs|
|--include-synced|/ --no-include-synced|
|Include|synced runs|
|--mark-synced|/ --no-mark-synced|
|Mark|runs as synced|
|--sync-all|Sync all runs|
|--clean|Delete synced runs|
|--clean-old-hours|Delete runs created before this many hours.|
|To|be used alongside --clean flag.|
|--clean-force|Clean without confirmation prompt.|
|--show|Number of runs to show|
|--help|Show this message and exit.|


# wandb verify

**Usage**

` wandb verify [OPTIONS]`

**Summary**

Verify your local instance


**Options**
| **Options** | **Description** |
|:--|:--|:--|
|--host|Test a specific instance of W&B|
|--help|Show this message and exit.|


