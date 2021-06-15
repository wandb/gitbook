# wandb.data\_types.BoundingBoxes2D

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.32/wandb/sdk/data_types.py#L1360-L1619)

Wandb class for logging 2D bounding boxes on images, useful for tasks like object detection

```python
BoundingBoxes2D(
    val: dict,
    key: str
) -> None
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>val</code>
      </td>
      <td style="text-align:left">
        <p>(dictionary) A dictionary of the following form:
          <br />
          <br />&quot;box_data&quot;: (list of dictionaries) One dictionary for each bounding
          box, containing:
          <br />
          <br />&quot;position&quot;: (dictionary) the position and size of the bounding
          box, in one of two formats.
          <br />Note that boxes need not all use the same format.
          <br />{&quot;minX&quot;, &quot;minY&quot;, &quot;maxX&quot;, &quot;maxY&quot;}:
          (dictionary) A set of coordinates defining the upper and lower bounds of
          the box (the bottom left and top right corners)
          <br />{&quot;middle&quot;, &quot;width&quot;, &quot;height&quot;}: (dictionary)
          A set of coordinates defining the center and dimensions of the box, with
          &quot;middle&quot; as a list [x, y] for the center point and &quot;width&quot;
          and &quot;height&quot; as numbers
          <br />
          <br />&quot;domain&quot;: (string, optional) One of two options for the bounding
          box coordinate domain</p>
        <p>By default, or if no argument is passed, the coordinate domain is assumed
          to be relative to the original image, expressing this box as a fraction
          or percentage of the original image. This means all coordinates and dimensions
          passed into the &quot;position&quot; argument are floating point numbers
          between 0 and 1.
          <br />&quot;pixel&quot;: The coordinate domain is set to the pixel space. This
          means all coordinates and dimensions passed into &quot;position&quot; are
          integers within the bounds of the image dimensions.
          <br />
          <br />&quot;class_id&quot;: (integer) The class label id for this box
          <br />
          <br />&quot;scores&quot;: (dictionary of string to number, optional) A mapping
          of named fields to numerical values (float or int), can be used for filtering
          boxes in the UI based on a range of values for the corresponding field
          <br
          />
          <br />&quot;box_caption&quot;: (string, optional) A string to be displayed as
          the label text above this box in the UI, often composed of the class label,
          class name, and/or scores
          <br />
          <br />&quot;class_labels&quot;: (dictionary, optional) A map of integer class
          labels to their readable class names</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>key</code>
      </td>
      <td style="text-align:left">(string) The readable name or id for this set of bounding boxes (e.g.
        predictions, ground_truth)</td>
    </tr>
  </tbody>
</table>

#### Examples:

Log a set of predicted and ground truth bounding boxes for a given image

```python
class_labels = {
    0: "person",
    1: "car",
    2: "road",
    3: "building"
}
img = wandb.Image(image, boxes={
    "predictions": {
        "box_data": [
            {
                # one box expressed in the default relative/fractional domain
                "position": {
                    "minX": 0.1,
                    "maxX": 0.2,
                    "minY": 0.3,
                    "maxY": 0.4
                },
                "class_id" : 1,
                "box_caption": class_labels[1],
                "scores" : {
                    "acc": 0.2,
                    "loss": 1.2
                }
            },
            {
                # another box expressed in the pixel domain
                "position": {
                    "middle": [150, 20],
                    "width": 68,
                    "height": 112
                },
                "domain" : "pixel",
                "class_id" : 3,
                "box_caption": "a building",
                "scores" : {
                    "acc": 0.5,
                    "loss": 0.7
                }
            },
            ...
            # Log as many boxes an as needed
        ],
        "class_labels": class_labels
    },
    # Log each meaningful group of boxes with a unique key name
    "ground_truth": {
    ...
    }
})

wandb.log({"driving_scene": img})
```

Prepare an image with bounding boxes to be added to a wandb.Table

```python
raw_image_path = "sample_image.png"

class_set = wandb.Classes([
    {"name" : "person", "id" : 0},
    {"name" : "car", "id" : 1},
    {"name" : "road", "id" : 2},
    {"name" : "building", "id" : 3}
])

image_with_boxes = wandb.Image(raw_image_path, classes=class_set,
    boxes=[...identical to previous example...])
```

## Methods

### `type_name` <a id="type_name"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/sdk/data_types.py#L1536-L1538)

```python
@classmethod
type_name() -> str
```

### `validate` <a id="validate"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/sdk/data_types.py#L1540-L1601)

```python
validate(
    val: dict
) -> bool
```

