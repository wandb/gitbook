# Launch Agents and Queues

{% hint style="danger" %}
This new product is in beta and under active development. Please message support@wandb.com with questions and suggestions.
{% endhint %}

Launch agents allow you and other members of your team to queue runs to be executed on remote infrastructure. Agents can be run on your machine or deployed via a container to the cloud to listen for new jobs pushed to your queues.

## Launch queues

Runs can be pushed to launch queues to be run by an agent. Each project has one `default` queue that cannot be deleted, and you can create more queues within a project by clicking the **Create queue** button on the launch queues page for a project.

Within a queue, runs are executed in sequential order. Runs can be queued via the wandb CLI or from the web UI, and runs can be deleted from the launch queues page.

## Launch agents

Agents are processes that poll launch queues and execute the jobs (or dispatch them to external services to be executed) in order. Agents typically execute runs asynchronously, so a single run shouldn't block the agent's poll-execution loop. Each agent can listen to multiple launch queues.

For details on how to run a launch agent locally, see the `wandb launch-agent` [CLI reference](../../ref/cli/wandb-launch-agent.md).

### Deployable agents

The launch agent can be deployed to Kubernetes using the provided deployment files found [here](https://github.com/wandb/wandb/tree/master/wandb/sdk/launch/deploys). Provide your API key, target project and maximum number of concurrent jobs in the launch configuration file. Check the launch-agent.yaml file for the name of the container that will be deployed.

