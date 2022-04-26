# wandb launch-agent

**Usage**

`wandb launch-agent [OPTIONS] PROJECT`

**Summary**

_Experimental feature._ Run a launch agent to poll for queued runs. `PROJECT` is the project to poll.

**Options**

| **Option**     | **Description**                                                                                                |
| -------------- | -------------------------------------------------------------------------------------------------------------- |
| -e, --entity   | The entity to use to run the agent. Defaults to current logged-in user.                                        |
| -q, --queues   | The queue or queues (comma-separated, e.g. `queue1,queue2`) to poll. Defaults to queue `default`.              |
| -j, --max-jobs | The maximum number of launch jobs this agent can run in parallel. Defaults to 1. Set to -1 for no upper limit. |
| --help         | Show this message and exit.                                                                                    |
