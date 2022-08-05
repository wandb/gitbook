# Launch with Amazon SageMaker

{% hint style="danger" %}
This new product is in beta and under active development. Please message support@wandb.com with questions and suggestions.
{% endhint %}

[Sagemaker ](https://aws.amazon.com/pm/sagemaker/?trk=8987dd52-6f33-407a-b89b-a7ba025c913c\&sc\_channel=ps\&sc\_campaign=acquisition\&sc\_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Machine%20Learning|Sagemaker|US|EN|Text\&s\_kwcid=AL!4422!3!532502995192!e!!g!!sagemaker\&ef\_id=CjwKCAjwopWSBhB6EiwAjxmqDXI888l7VXCP\_vdFZYDAxR0uxs0WeD0vbFrsYgQDEqRj0wDPFoT6BxoC5PoQAvD\_BwE:G:s\&s\_kwcid=AL!4422!3!532502995192!e!!g!!sagemaker)is Amazon's ML development platform. Using Launch you can build and execute training jobs on Sagemaker.

## SageMaker quickstart

### Requirements

* The system should be configured with AWS by running the command: `aws configure`
* Create an ECR repository and give access to the AWS user associated with the machine that will run launch.
* Create a Sagemaker execution role with access to pull images from the ECR Repository, and if you intend on outputting data to S3, access to push to the desired S3 bucket.
* Make sure you have the latest version of wandb and you have Launch enabled by adding `instant replay` to your W\&B profile.



You should now be able to run `wandb launch <URI> --resource sagemaker --resource-args <JSON args>`, or select the GCP Vertex resource option from the web UI.

## Resource args reference

The arguments below should be supplied as a JSON dictionary under the key `sagemaker` and passed in via `--resource-args` or via the web UI. **Arguments in bold are required.**&#x20;

| Name                  | Type   | Description                                                                    |
| --------------------- | ------ | ------------------------------------------------------------------------------ |
| **EcrRepoName**       | string | An ECR repository that wandb launch can send the built container image to.     |
| region                | string | The AWS region to use, can override the default found in `~/.aws/config`       |
| profile               | string | the local aws profile configuration to use, defaults to `default`              |
| **OutputDataConfig**  | dict   | configuration indicating where to send training job output data                |
| **ResourceConfig**    | dict   | configuration indicating the resource to use to run the training job           |
| **StoppingCondition** | dict   | dictionary indicating the conditions which will cause the training job to stop |
| **RoleArn**           | string | the AWS role used to execute training jobs on Sagemaker.                       |

Sagemaker training jobs are highly configurable and additional resource arguments can be provided. For an exhaustive list of all possible arguments see [Sagemaker API reference](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API\_CreateTrainingJob.html).

Note that if the `AlgorithmSpecification` argument is provided and contains a`TrainingImage` key the value of this key will be used without a build step.
