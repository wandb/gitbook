# ImageMask



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.33/wandb/sdk/data_types.py#L1182-L1357)



Wandb class for image masks or overlays, useful for tasks like semantic segmentation.

```python
ImageMask(
    val: dict,
    key: str
) -> None
```





| Arguments |  |
| :--- | :--- |
|  `val` |  (dictionary) One of these two keys to represent the image: mask_data : (2D numpy array) The mask containing an integer class label for each pixel in the image path : (string) The path to a saved image file of the mask class_labels : (dictionary of integers to strings, optional) A mapping of the integer class labels in the mask to readable class names. These will default to class_0, class_1, class_2, etc. |
|  `key` |  (string) The readable name or id for this mask type (e.g. predictions, ground_truth) |



#### Examples:

Log a mask overlay for a given image
```python
predicted_mask = np.array([[1, 2, 2, ... , 3, 2, 1], ...])
ground_truth_mask = np.array([[1, 1, 1, ... , 2, 3, 1], ...])

class_labels = {
    0: "person",
    1: "tree",
    2: "car",
    3: "road"
}

masked_image = wandb.Image(image, masks={
    "predictions": {
        "mask_data": predicted_mask,
        "class_labels": class_labels
    },
    "ground_truth": {
        "mask_data": ground_truth_mask,
        "class_labels": class_labels
    }
}
wandb.log({"img_with_masks" : masked_image})
```

Prepare an image mask to be added to a wandb.Table
```python
raw_image_path = "sample_image.png"
predicted_mask_path = "predicted_mask.png"
class_set = wandb.Classes([
    {"name" : "person", "id" : 0},
    {"name" : "tree", "id" : 1},
    {"name" : "car", "id" : 2},
    {"name" : "road", "id" : 3}
])
masked_image = wandb.Image(raw_image_path, classes=class_set,
    masks={"prediction" : {"path" : predicted_mask_path}})
```


## Methods

<h3 id="type_name"><code>type_name</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/sdk/data_types.py#L1327-L1329)

```python
@classmethod
type_name() -> str
```




<h3 id="validate"><code>validate</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/sdk/data_types.py#L1331-L1357)

```python
validate(
    val: dict
) -> bool
```






