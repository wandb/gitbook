---
description: 'Track artifacts saved outside of W&B, for example in a GCP or S3 bucket'
---

# Artifacts by Reference

### Visualize Training Data from the Cloud <a id="visualize-training-data-from-the-cloud"></a>

How can you use W&B to track and visualize data which lives in a remote storage bucket? This is a walkthrough of [Dataset and Predictions visualization](https://docs.wandb.ai/datasets-and-predictions) for a tiny dataset of audio files stored on the Google Cloud Platform \(GCP\). The dataset in this example consists of a few original songs \(.wav files\) and their synthesized/regenerated versions \(the same melody played by a different instrument, see [this report](https://wandb.ai/stacey/cshanty/reports/Visualize-Audio-Data-in-W-B--Vmlldzo1NDMxMDk) for details\). If you're using AWS or a different cloud provider, the only changes to this report are the provider-specific syntax for bucket setup and file upload/download.

### Setup <a id="setup"></a>

If you're not already training on data from GCP storage:

* set up the correct permissions: enable your local environment \(remote GPU box, notebook, etc\) to connect with your remote storage: have the necessary read/write permissions to the specific cloud storage buckets you'll be using
* install the right libraries \(e.g. google.cloud.storage\) and download the right access keys to enable you to call read/write commands from your scripts and local environment
* organize the metadata and directory structure in your bucket so you can associate the right information/labels with the right files: I use a CSV file \(song\_metadata.csv\) listing all the filenames in my dataset alongside ids and associated metadata \(location, species, date for each recording\)
* enable object versioning \(optional\): If you want to be able to recover the contents of files after you change or delete them, enable [object versioning](https://cloud.google.com/storage/docs/object-versioning) before uploading any files to your remote bucket. In GCP, you can [control object versioning](https://cloud.google.com/storage/docs/using-object-versioning#gsutil) via gsutil or the google.cloud.storage API.

### Create an artifact by reference <a id="create-an-artifact-by-reference"></a>

Version data from a remote file system in your project via external [reference](https://wandb.ai/stacey/cshanty/artifacts/raw_data/sample_songs/1e69acd4ac6b3c35f24f/files). The change from [logging a regular W&B Artifact](https://docs.wandb.ai/artifacts) is minimal: instead of adding a local path with `artifact.add_artifact([your local file path])`, add a remote path \(generally a URI\) with `artifact.add_reference([your remote path])`

```text
import wandbrun = wandb.init(project=﻿"songs"﻿, job_type=﻿"upload"﻿)# path to my remote data directory in Google Cloud Storagebucket = "gs://wandb-artifact-refs-public-test/whalesong"# create a regular artifactdataset_at = wandb.Artifact(﻿'sample_songs'﻿,﻿type﻿=﻿"raw_data"﻿)# creates a checksum for each file and adds a reference to the bucket# instead of uploading all of the contentsdataset_at.add_reference(bucket)run.log_artifact(dataset_at)
```

List of file paths and sizes in this reference bucket. Note that these are merely references to the contents, not actual files stored in W&B, so they are not available for download from this view

![](https://api.wandb.ai/files/stacey/images/projects/216402/a1438787.png)

### Change the contents of the remote bucket <a id="change-the-contents-of-the-remote-bucket"></a>

Let's say I add two new songs to my GCP bucket. Next time I call the artifact.add\_reference\(bucket\) command, wandb will detect and sync any changes, including edits to file contents. The [comparison page](https://wandb.ai/stacey/cshanty/artifacts/raw_data/sample_songs/c19f00a4b9f66270715f/files#1e69acd4ac6b3c35f24f) shows the file diff \(7 songs on the left in v1, 5 on the right in v0\). I can also update the aliases and take notes on each version to remind myself of the changes I've made.

![](https://api.wandb.ai/files/stacey/images/projects/216402/ff308f22.png)

![](https://api.wandb.ai/files/stacey/images/projects/216402/428e4d11.png)

### Download data from the cloud <a id="download-data-from-the-cloud"></a>

Of course you can still fetch the files from the reference artifact and use the data locally:

```text
import wandbrun = wandb.init(project=﻿"songs"﻿, job_type=﻿"show_samples"﻿)dataset_at = run.use_artifact(﻿"sample_songs:latest"﻿)songs_dir = dataset_at.download(﻿)# all files available locally in songs_dir
```

### Visualize data by reference <a id="visualize-data-by-reference"></a>

You can visualize data and predictions via reference paths \(URIs\) to remote storage. Set up a [dataset visualization table](https://docs.wandb.ai/datasets-and-predictions) to interact with your data: listen to audio samples, play large videos, see images, and more. With this approach, you don't need to fill up local storage, wait for files to download, open media in a different app, or navigate multiple windows/browser tabs of file directories.In this example, I've manually uploaded some marine mammal vocalizations to a public storage bucket on GCP. The full dataset is available from the [Watkins Marine Mammal Sound Database](https://cis.whoi.edu/science/B/whalesounds/index.cfm), and you can play the sample songs directly from W&B.

﻿[Interact with this example →](https://wandb.ai/stacey/cshanty/artifacts/raw_data/playable_songs/9310d05b6db672423a71/files/song_samples.table.json) ﻿

Press play/pause on any song and view additional metadata

![](https://api.wandb.ai/files/stacey/images/projects/216402/8ca0c14d.png)

#### Upload and visualize training results <a id="upload-and-visualize-training-results"></a>

Beyond input training data, you may want to visualize intermediate or final training results: model predictions over the course of training, examples generated with different hyperparameters, etc. You can join these to existing data tables to set up powerful interactive visualizations and analysis. The generated/synthetic songs in this example are local .wav files created in a Colab or my local dev environment. Each file is associated with the original song\_id and the target instrument.

#### Track any media created during training <a id="track-any-media-created-during-training"></a>

There are several ways to upload and version any files produced during model training or evaluation:

* ﻿[log a W&B Artifact](https://docs.wandb.ai/artifacts/dataset-versioning#25c79f05-174e-4d35-abda-e5c238b8d6d6) to track local files as they're created \(easiest strategy\)
* ﻿[log a W&B Artifact by reference](https://docs.wandb.ai/artifacts/api#adding-references) to track remote files:
  * upload any local files to a remote cloud bucket, two options:
    * programmatically \(probably via the cloud provider API, say google.cloud.storage\)
    * or manually via your browser UI
  * version any changes to the files
    * Make sure versioning is enabled in the remote cloud bucket. Since the contents of the files aren't stored in W&B, this is the only way to recover old versions after you change or delete them. Note that versioning needs to be enabled before uploading any files to the bucket, not after you've started using the bucket.
    * Call ref\_artifact.add\_reference\(bucket\) after any meaningful changes to the bucket, in order for W&B to pick up your remote changes and sync the latest version.

#### Interact with media stored in a remote bucket <a id="interact-with-media-stored-in-a-remote-bucket"></a>

﻿[Play some synthesized samples in a live project](https://wandb.ai/stacey/cshanty/artifacts/synth_ddsp/synth_samples/44d3c8c6539959ea7371/files/synth_song_samples.table.json)﻿﻿  
﻿[﻿](https://wandb.ai/stacey/cshanty/artifacts/synth_ddsp/synth_samples/44d3c8c6539959ea7371/files/synth_song_samples.table.json)﻿[﻿](https://wandb.ai/stacey/cshanty/artifacts/synth_ddsp/synth_samples/44d3c8c6539959ea7371/files/synth_song_samples.table.json)To see and interact with audio files, log them directly into a wandb.Table associated with an artifact. This is very similar to the regular scenario where you have local files/folders to sync as artifacts. The two key differences when visualizing remote files are:

![](https://api.wandb.ai/files/stacey/images/projects/216402/f649179f.png)

* reference remote artifacts: use dataset\_artifact.add\_reference\(remote\_bucket\) instead of dataset\_artifact.add\_dir\(local\_dir\) to track and version a collection of remote files
* walk the remote directory tree and construct paths to each media file: in order to visualize a piece of media such as an image, video, or song \(audio file\) in the browser, we need to wrap it in a wandb object of the matching type—in this case, wandb.Audio\(\). The wandb object takes in a file path to render the contents of the file. Since these files are not available locally, pass in the full path of the file in the remote bucket when creating the wandb object. We plan to simplify this syntax in the future.

Sample code for rendering the synthetic songs, assuming they've been uploaded to and stored in the remote bucket \(whether manually or programmatically\):

```text
import osimport wandbfrom google.cloud import storage﻿
run = wandb.init(project=﻿"songs"﻿, job_type=﻿"log_synth"﻿)# full path to the specific folder of synthetic songs# (note the "gs://" prefix for Google Storage)synth_songs_bucket = "gs://wandb-artifact-refs-public-test/whalesong/synth"# root of the remote bucket (note, no "gs://" prefix)bucket_root = "wandb-artifact-refs-public-test"dataset_at = wandb.Artifact(﻿'synth_songs'﻿,﻿type﻿=﻿"generated_data"﻿)﻿
# track all the files in the specific folder of synthetic songsdataset_at.add_reference(synth_songs_bucket)﻿
# iterate over locations in GCP from the root of the bucketbucket_iter = storage.Client(﻿)﻿.get_bucket(bucket_root)song_data = [﻿]# focus on the synth songs folderfor synth_song in bucket_iter.list_blobs(prefix=﻿"whalesong/synth"﻿)﻿:  # filter out any non-audio files  if not synth_song.name.endswith(﻿".wav"﻿)﻿:    continue    # add a reference path for each song  # song filenames have the form [string id]_[instrument].wav  song_name = synth_song.name.split(﻿"/"﻿)﻿[﻿-﻿1﻿]  song_path = os.path.join(synth_songs_bucket, song_name)  # create a wandb.Audio object to show the audio file  audio = wandb.Audio(song_path, sample_rate=﻿32﻿)  # extract instrument from the filename  orig_song_id, instrument = song_name.split(﻿"_"﻿)  song_data.append(﻿[orig_song_id, song_name, audio, instrument.split(﻿"."﻿)﻿[﻿0﻿]﻿]﻿)﻿
# create a table to hold audio samples and metadata in columnstable = wandb.Table(data=song_data,                    columns=﻿[﻿"song_id"﻿, "song_name"﻿, "audio"﻿, "instrument"﻿]﻿)# log the table via a new artifactsongs_at = wandb.Artifact(﻿"synth_samples"﻿, type﻿=﻿"synth_ddsp"﻿)songs_at.add(table, "synth_song_samples"﻿)run.log_artifact(songs_at)
```

#### Analyze remote media dynamically <a id="analyze-remote-media-dynamically"></a>

Once the remote media is logged to a visualization table in an artifact, you can dynamically sort, group, filter, query, and otherwise process individual tables, as well as join across tables. Read more in [this report](https://wandb.ai/stacey/cshanty/reports/Visualize-Audio-Data-in-W-B--Vmlldzo1NDMxMDk), and check out this live comparison of original and synthesized songs side-by-side:

﻿[Live example →](https://wandb.ai/stacey/cshanty/artifacts/analysis/synth_summary/0af204744b98ae0469cf/files/synth_explore.joined-table.json) ﻿﻿  


![](https://api.wandb.ai/files/stacey/images/projects/216402/9043e92e.png)

