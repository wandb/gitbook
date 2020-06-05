# Artifacts API

Use W&B Artifacts for dataset tracking and model versioning. Initialize a run, create an artifact, and then use it in another part of your workflow. You can use artifacts to track and save files, or track external URIs.

## 1. Initialize a run

To track a step of your pipeline, initialize a run in your script. Specify a string for **job\_type** to differentiate different pipeline steps— preprocessing, training, evaluation, etc. If you've never instrumented a run with W&B, we have more detailed guidance for experiment tracking in our [Python Library](../library/) docs.

```python
run = wandb.init(job_type='train')
```

## 2. Create an artifact

An artifact is like a folder of data, with contents that are actual files stored in the artifact or references to external URIs. To create an artifact, log it as the output of a run. Specify a string for **type** to differentiate different artifacts— dataset, model, result etc. Give this artifact a **name**, like `bike dataset`, to help you remember what is inside the artifact. In a later step of your pipeline, you can use this name along with a version like `bike dataset:v1`  to download this artifact.

When you call **log\_artifact**, we check to see if the contents of the artifact has changed, and if so we automatically create a new version of the artifact: v0, v1, v2 etc. The most recent version of an artifact can be accessed with `artifact-name:latest`. 

**wandb.Artifact\(\)**

* **type \(str\)**: differentiate kinds of artifacts like dataset, model, result
* **name \(str\)**: give your artifact a unique name, used when you reference the artifact elsewhere
* **description \(str, optional\)**: Free text displayed next to the artifact version in the UI
* **metadata \(dict, optional\)**: Structured data associated with the artifact, for example class distribution of a dataset. As we build out the web interface, you'll be able to use this data to query and make plots.

```python
artifact = wandb.Artifact(type='dataset', name='bike-dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Mark this artifact version as the output of this run
run.log_artifact(artifact)
```

## 3. Use an artifact

You can use an artifact as input to a run. For example, we could take `bike dataset:latest` , the most recently logged version of `bike dataset`, and use it in the next script in our pipeline. When you call **use\_artifact**, your script queries W&B to find that named artifact and marks it as input to the run.

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact(type='dataset', name='bike-dataset:latest')

# Download the artifact's contents
artifact_dir = artifact.download()

# Path
```

**Using an artifact that has not been logged**  
You can also construct an artifact object and pass it to **use\_artifact**. We check if the artifact already exists in W&B, and if not it creates a new artifact. This is idempotent— you can pass an artifact to use\_artifact as many times as you like, and we'll deduplicate it as long as the contents stay the same.

```python
artifact = wandb.Artifact(type='model', name='bike-model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

## Versions and aliases

When you log an artifact for the first time, we create version **v0**. When you log again to the same artifact, we checksum the contents, and if the artifact has changed we save a new version **v1**. Each time you log a new artifact version, we update the alias **latest** to point to the most recent version of that artifact. 

For example, if you want your training script to always pull the most recent version of a dataset, specify **latest** when you use that artifact.

```python
artifact = run.use_artifact(type='dataset', name='bike-dataset:latest')
```

You can also apply a custom alias to an artifact version. For example, if you want to mark which model is in production, you could add the string **production** as an alias when you log the model artifact.

```python
artifact = wandb.Artifact(type='model', name='bike-model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['production'])
```

## Files and references

Track external URIs or files in the contents of artifacts. Use **name** to specify an optional file name, or a file path prefix if you're adding a directory. 

```python
# Add a URI reference to an artifact
artifact.add_reference(uri, name='optional-name')

# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
artifact.new_file(name)
```

### Examples of using artifact file names

For the following examples, assume we have a project directory with these files:

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- tmp
    +-- model.h5
|-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;/tmp/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">1.png2.png</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/1.png</p>
        <p>images/2.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>



