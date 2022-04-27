# Launch with GCP Vertex AI

[Vertex AI](https://cloud.google.com/vertex-ai) is Google Cloud's ML development platform. With Launch, you can build and execute training jobs and run them with Vertex on the compute infrastructure of your choice.

## Vertex quickstart

### Requirements

* Make sure you have the [`gcloud` command-line tool](https://cloud.google.com/sdk/docs/install) installed, Vertex AI enabled in your Google Cloud console, and the [GCP AI Platform Python client libraries](https://cloud.google.com/vertex-ai/docs/start/client-libraries) installed.
* Enable and create an artifact repository in [GCP Artifact Registry](https://cloud.google.com/artifact-registry). This repository is where the Docker image built by Launch containing your training job will be pushed. It must be in the same compute region where you want your job to run.
* Enable and create a storage bucket in [GCP Cloud Storage](https://cloud.google.com/storage). This bucket is where data and model resources associated with your training runs will be stored. It must be in the same region where you want your job to run.
* Make sure you have the latest version of wandb and you have Launch enabled by adding `instant replay` to your W\&B profile.

You should now be able to run `wandb launch <URI> --resource gcp-vertex --resource-args <JSON args>`, or select the GCP Vertex resource option from the web UI.

## Resource args reference

The arguments below should be supplied as a JSON dictionary under the key `gcp_vertex` and passed in via `--resource-args` or via the web UI. **Arguments in bold are required.**&#x20;

| Name                | Type    | Description                                                                                                                      |
| ------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------- |
| gcp\_config         | str     | Named [gcloud configuration](https://cloud.google.com/sdk/gcloud/reference/config/configurations). Defaults to `default` config. |
| gcp\_project        | str     | GCP project. Defaults to current project in config.                                                                              |
| gcp\_region         | str     | Region. Defaults to current region in config.                                                                                    |
| job\_name           | str     | Name of Vertex job. Defaults to `{wandb project}_{job timestamp}`.                                                               |
| **staging\_bucket** | **str** | **Cloud Storage bucket for storage of resources associated with run. Required.**                                                 |
| **artifact\_repo**  | **str** | **Artifact Registry repository for image. Required.**                                                                            |
| docker\_host        | str     | Docker host for image. Defaults to `{region}-docker.pkg.dev` for the active region.                                              |
| machine\_type       | str     | Type of machine to run the job on. Defaults to n1-standard-4.                                                                    |
| accelerator\_type   | str     | Accelerator type to use. Defaults to ACCELERATOR\_TYPE\_UNSPECIFIED                                                              |
| accelerator\_count  | int     | Number of accelerators to use. Defaults to 0.                                                                                    |
