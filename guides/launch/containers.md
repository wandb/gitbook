# Containerizing with Launch

Launch abstracts away the details of executing runs by packaging up everything needed for a run into a portable container environment. You can either have Launch build a container for you, or you can supply your own image and Launch will run it on your chosen infrastructure.

## Building a container with Launch

### Building with Docker

To create a reproducible, portable environment for a run, Launch packages the run into a Docker container. Prerequisites include:

* Docker must be installed on your machine
* When reproducing an existing wandb run, that run must have a git repository and commit version associated in the run overview page. You can also launch from a remote git repository, or from a local directory containing your code. You must have permissions to the relevant repository.
* The root directory of your code repository should have either a `requirements.txt` (pip) or `environment.yml` (conda) present. If neither is present and you are launching from an existing run, Launch will read all accessible pip packages in the run's original environment.
* Install [BuildX](https://github.com/docker/buildx) to enable dependency caching (e.g. of individual pip requirements) as well as Docker layer caching in your builds.

Some non-requirements:

* A clean git commit: Launch automatically applies the `diff.patch` recorded from the original run, which captures any changes since the last commit. If you override the commit version in your Launch configuration, this will be ignored.
* If you used `use_artifact` to pull in your dataset or other input files, your data does not need to be local or preloaded.

## Bring your own container

Launch also accepts pre-built containers already containing the code and runtime environment for a run. To use a pre-built container, run `wandb launch --docker-image <image>`, or schedule a job via the web UI as follows:

![](<../../.gitbook/assets/image (177).png>)

The image can be local or hosted on a remote registry (in which case you must have Docker permissions to pull from that registry).
