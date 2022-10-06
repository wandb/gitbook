# Project



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1446-L1528)



A project is a namespace for runs.

```python
Project(
    client, entity, project, attrs
)
```







| Attributes |  |
| :--- | :--- |



## Methods

<h3 id="artifacts_types"><code>artifacts_types</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1479-L1481)

```python
artifacts_types(
    per_page=50
)
```




<h3 id="display"><code>display</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L959-L970)

```python
display(
    height=420, hidden=(False)
) -> bool
```

Display this object in jupyter


<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L955-L957)

```python
snake_to_camel(
    string
)
```




<h3 id="sweeps"><code>sweeps</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1483-L1528)

```python
sweeps()
```




<h3 id="to_html"><code>to_html</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1463-L1471)

```python
to_html(
    height=420, hidden=(False)
)
```

Generate HTML containing an iframe displaying this project




