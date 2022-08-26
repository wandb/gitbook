# Construct an artifact

Use the Weights and Biases SDK to construct an artifact during or outside of a Weights and Biases Run. Add files, directories, URIs, and files from parallel runs to artifacts. Once a file or URI is added, save your artifact to Weights and Biases with a Weights and Biases Run. For information on how to track an external file outside of a Weights and Biases run, see [Track external files](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/track-external-files).

### How to construct an artifact

Constructing a Weights and Biases artifact within a run is a three step process:

#### 1. Create an artifact Python object with `wandb.Artifact()`

Initialize the [`wandb.Artifact()`](https://docs.wandb.ai/ref/python/artifact) class to create an artifact. Specify the following parameters:

* **Name**: Specify a name for your artifact. The name should be unique, descriptive, and easy to remember. Use an artifacts name to both: identify the artifact in the Weights and Biases App UI and when you want to use that artifact.
* **Type**: Provide a type. The type should be simple, descriptive and correspond to a single step of your machine learning pipeline. Common artifact types include `'dataset'` or `'model'`.

{% hint style="info" %}
Artifacts can not have the same name, even if you specify a different type for the types parameter. In other words, you can not create an artifact named ‘cats’ of type ‘dataset’ and another artifact with the same name of type ‘model’.
{% endhint %}

You can optionally provide a description and metadata when you initialize an artifact object. For more information on available attributes and parameters, see [wandb.Artifact](https://docs.wandb.ai/ref/python/artifact) Class definition in the Python SDK Reference Guide.

The proceeding example demonstrates how to create a dataset artifact:

```python
import wandb

artifact = wandb.Artifact(name='<replace>', type='<replace>')
```

Replace the string arguments in the preceding code snippet with your own name and type.

#### 2. Add one more files to the artifact

Add files, directories, external URI references (such as Amazon S3) and more with artifact methods. For example, to add a single text file, use the [`add_file`](https://docs.wandb.ai/ref/python/artifact#add\_file) method:

```python
artifact.add_file(local_path='hello_world.txt', name='optional-name')
```

You can also add multiple files with the [`add_dir`](https://docs.wandb.ai/ref/python/artifact#add\_dir) method. For more information on how to add files, see [Update an artifact](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/update-an-artifact).

#### 3. Save your artifact to the Weights and Biases server

Finally, save your artifact to the Weights and Biases server. Artifacts are associated with a run. Therefore, use a run objects [`log_artifact()`](https://docs.wandb.ai/ref/python/run#log\_artifact) method to save the artifact.

```python
# Create a W&B Run. Replace 'job-type'.
run = wandb.init(project="artifacts-example", job_type='job-type')

run.log_artifact(artifact)
```

You can optionally construct an artifact outside of a Weights and Biases run. For more information, see [Track external files](https://app.gitbook.com/o/-Lr2SEfv2R3GSuF1kZCt/s/-Lqya5RvLedGEWPhtkjU-1972196547/\~/changes/j1B9n6G73J5mTKwAVy6u/guides/artifacts/track-external-files).

{% hint style="warning" %}
Calls to `log_artifact` are performed asynchronously for performant uploads. This can cause surprising behavior when logging artifacts in a loop. For example:

```python
for i in range(10):
    a = wandb.Artifact('race', type='dataset', metadata={
        "index": i,
    })
    # ... add files to artifact a ...
    run.log_artifact(a)
```

The artifact version **v0** is NOT guaranteed to have an index of 0 in its metadata, as the artifacts may be logged in an arbitrary order.
{% endhint %}

### Add files to an artifact

The following sections demonstrate how to construct artifacts with different file types and from parallel runs.

For the following examples, assume you have a project directory with multiple files and a directory structure:

```
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

### Add a single file

The proceeding code snippet demonstrates how to add a single, local file to your artifact:

```python
# Add a single file
artifact.add_file(local_path='path/file.format')
```

For example, suppose you had a file called `'file.txt'` in your working local directory.

```python
artifact.add_file('path/file.txt') # Added as `file.txt'
```

The artifact now has the following content:

```
file.txt
```

Optionally, pass the desired path within the artifact for the `name` parameter.

```python
artifact.add_file('path/file.format', name='new/path/file.format') 
```

The artifact is stored as:

```
new/path/file.txt
```

| API Call                                                  | Resulting artifact |
| --------------------------------------------------------- | ------------------ |
| `artifact.add_file('model.h5')`                           | model.h5           |
| `artifact.add_file('checkpoints/model.h5')`               | model.h5           |
| `artifact.add_file('model.h5', name='models/mymodel.h5')` | models/mymodel.h5  |

### Add multiple files

The proceeding code snippet demonstrates how to add an entire, local directory to your artifact:

```python
# Recursively add a directory
artifact.add_dir(local_path='path/file.format', name='optional-prefix')
```

The proceeding API calls produce the proceeding artifact content:

| API Call                                    | Resulting artifact                                     |
| ------------------------------------------- | ------------------------------------------------------ |
| `artifact.add_dir('images')`                | <p><code>cat.png</code></p><p><code>dog.png</code></p> |
| `artifact.add_dir('images', name='images')` | `name='images')images/cat.pngimages/dog.png`           |
| `artifact.new_file('hello.txt')`            | `hello.txt`                                            |

### Add a URI reference

Artifacts track checksums and other information for reproducibility if the URI has a scheme that Weights and Biases library knows how to handle.

Add an external URI reference to an artifact with the [`add_reference`](https://docs.wandb.ai/ref/python/artifact#add\_reference) method. Replace the `'uri'` string with your own URI. Optionally pass the desired path within the artifact for the name parameter.

```python
# Add a URI reference
artifact.add_reference(uri='uri', name='optional-name')
```

Artifacts currently support the following URI schemes:

* `http(s)://`: A path to a file accessible over HTTP. The artifact will track checksums in the form of etags and size metadata if the HTTP server supports the `ETag` and `Content-Length` response headers.
* `s3://`: A path to an object or object prefix in S3. The artifact will track checksums and versioning information (if the bucket has object versioning enabled) for the referenced objects. Object prefixes are expanded to include the objects under the prefix, up to a maximum of 10,000 objects.
* `gs://`: A path to an object or object prefix in GCS. The artifact will track checksums and versioning information (if the bucket has object versioning enabled) for the referenced objects. Object prefixes are expanded to include the objects under the prefix, up to a maximum of 10,000 objects.

The proceeding API calls will produce the proceeding artifacts:

| API call                                                                      | Resulting artifact contents                                          |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `artifact.add_reference('s3://my-bucket/model.h5')`                           | `model.h5`                                                           |
| `artifact.add_reference('s3://my-bucket/checkpoints/model.h5')`               | `model.h5`                                                           |
| `artifact.add_reference('s3://my-bucket/model.h5', name='models/mymodel.h5')` | `models/mymodel.h5`                                                  |
| `artifact.add_reference('s3://my-bucket/images')`                             | <p><code>cat.png</code></p><p><code>dog.png</code></p>               |
| `artifact.add_reference('s3://my-bucket/images', name='images')`              | <p><code>images/cat.png</code></p><p><code>images/dog.png</code></p> |

### Add files to artifacts from parallel runs

For large datasets or distributed training, multiple parallel runs might need to contribute to a single artifact.

```python
import wandb
import time

# We will use ray to launch our runs in parallel
# for demonstration purposes. You can orchestrate
# your parallel runs however you want.
import ray

ray.init()

artifact_type = "dataset"
artifact_name = "parallel-artifact"
table_name = "distributed_table"
parts_path = "parts"
num_parallel = 5

# Each batch of parallel writers should have its own
# unique group name.
group_name = "writer-group-{}".format(round(time.time()))

@ray.remote
def train(i):
  """
  Our writer job. Each writer will add one image to the artifact.
  """
  with wandb.init(group=group_name) as run:
    artifact = wandb.Artifact(name=artifact_name, type=artifact_type)
    
    # Add data to a wandb table. In this case we use example data
    table = wandb.Table(columns=["a", "b", "c"], data=[[i, i*2, 2**i]])
    
    # Add the table to folder in the artifact
    artifact.add(table, "{}/table_{}".format(parts_path, i))
    
    # Upserting the artifact creates or appends data to the artifact
    run.upsert_artifact(artifact)
  
# Launch your runs in parallel
result_ids = [train.remote(i) for i in range(num_parallel)]

# Join on all the writers to make sure their files have
# been added before finishing the artifact. 
ray.get(result_ids)

# Once all the writers arefinished, finish the artifact
# to mark it ready.
with wandb.init(group=group_name) as run:
  artifact = wandb.Artifact(artifact_name, type=artifact_type)
  
  # Create a "PartitionTable" pointing to the folder of tables
  # and add it to the artifact.
  artifact.add(wandb.data_types.PartitionedTable(parts_path), table_name)
  
  # Finish artifact finalizes the artifact, disallowing future "upserts"
  # to this version.
  run.finish_artifact(artifact)
```
