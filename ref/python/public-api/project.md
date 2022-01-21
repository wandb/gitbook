# wandb.apis.public.Project

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L1207-L1289)

A project is a namespace for runs.

```python
Project(
    client, entity, project, attrs
)
```

## Methods

### `artifacts_types` <a href="#artifacts_types" id="artifacts_types"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L1240-L1242)

```python
artifacts_types(
    per_page=50
)
```

### `display` <a href="#display" id="display"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L777-L788)

```python
display(
    height=420, hidden=(False)
) -> bool
```

Display this object in jupyter

### `snake_to_camel` <a href="#snake_to_camel" id="snake_to_camel"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L773-L775)

```python
snake_to_camel(
    string
)
```

### `sweeps` <a href="#sweeps" id="sweeps"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L1244-L1289)

```python
sweeps()
```

**Example**:

Find sweeps in `my_project` to get a list of all sweeps from this project:

```python
sweeps = wandb.Api().project('my_project').sweeps()
```

### `to_html` <a href="#to_html" id="to_html"></a>

[View source](https://www.github.com/wandb/client/tree/v0.12.9/wandb/apis/public.py#L1224-L1232)

```python
to_html(
    height=420, hidden=(False)
)
```

Generate HTML containing an iframe displaying this project
