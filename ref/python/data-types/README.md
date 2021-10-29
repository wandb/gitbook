# data-types

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/__init__.py)



Wandb has special data types for logging rich visualizations.


All of the special data types are subclasses of WBValue. All of the data types
serialize to JSON, since that is what wandb uses to save the objects locally
and upload them to the W&B server.

## Classes

[`class Audio`](./audio.md): Wandb class for audio clips.

[`class BoundingBoxes2D`](./boundingboxes2d.md): Wandb class for logging 2D bounding boxes on images, useful for tasks like object detection

[`class Graph`](./graph.md): Wandb class for graphs

[`class Histogram`](./histogram.md): wandb class for histograms.

[`class Html`](./html.md): Wandb class for arbitrary html

[`class Image`](./image.md): Wandb class for images.

[`class ImageMask`](./imagemask.md): Wandb class for image masks or overlays, useful for tasks like semantic segmentation.

[`class Molecule`](./molecule.md): Wandb class for Molecular data

[`class Object3D`](./object3d.md): Wandb class for 3D point clouds.

[`class Plotly`](./plotly.md): Wandb class for plotly plots.

[`class Table`](./table.md): The Table class is used to display and analyze tabular data.

[`class Video`](./video.md): Wandb representation of video.

