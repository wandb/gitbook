# Create a custom alias

Use aliases as pointers to specific versions. By default, `Run.log_artifact` adds the `latest` alias to the logged version.

An artifact version `v0` is created and attached to your artifact when you log an artifact for the first time. Weights & Biases checksums the contents when you log again to the same artifact. If the artifact changed, Weights & Biases saves a new version `v1`.

For example, if you want your training script to pull the most recent version of a dataset, specify `latest` when you use that artifact. The proceeding code example demonstrates how to download a recent dataset artifact named `bike-dataset` that has an alias, `latest`:

```python
import wandb

run = wandb.init(project='<example-project>')

artifact = run.use_artifact('bike-dataset:latest')

artifact.download()
```

You can also apply a custom alias to an artifact version. For example, if you want to mark that model checkpoint is the best on the metric AP-50, you could add the string `'best-ap50'` as an alias when you log the model artifact.

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```
