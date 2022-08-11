# Launch Agents and Queues

{% hint style="danger" %}
### <mark style="color:red;">**Beta product in active development**</mark>

<mark style="color:red;">**Interested in Launch? Reach out to your account team to talk about joining the customer pilot program for W\&B Launch.**</mark>

<mark style="color:red;">**Pilot customers need to use AWS EKS or SageMaker to qualify for the beta program. We ultimately plan to support additional platforms.**</mark>
{% endhint %}

## Launch queues

Runs can be pushed to launch queues to be run by an agent. Each project has one `default` queue that cannot be deleted, and you can create more queues within a project by clicking the **Create queue** button on the launch queues page for a project.

Within a queue, runs are executed in sequential order. Runs can be queued via the wandb CLI or from the web UI, and runs can be deleted from the launch queues page.

## Launch agents

Agents are processes that poll launch queues and execute the jobs (or dispatch them to external services to be executed) in order. Agents typically execute runs asynchronously, so a single run shouldn't block the agent's poll-execution loop. Each agent can listen to multiple launch queues.

For details on how to run a launch agent locally, see the `wandb launch-agent` [CLI reference](../../ref/cli/wandb-launch-agent.md).

### Deployable agents

The launch agent can be deployed to Kubernetes using the provided deployment files found [here](https://github.com/wandb/wandb/tree/master/wandb/sdk/launch/deploys). Provide your API key, target project and maximum number of concurrent jobs in the launch configuration file. Check the launch-agent.yaml file for the name of the container that will be deployed.
