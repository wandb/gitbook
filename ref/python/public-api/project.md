# Project



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1408-L1490)



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

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1441-L1443)

```python
artifacts_types(
    per_page=50
)
```




<h3 id="display"><code>display</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L935-L946)

```python
display(
    height=420, hidden=(False)
) -> bool
```

Display this object in jupyter


<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L931-L933)

```python
snake_to_camel(
    string
)
```




<h3 id="sweeps"><code>sweeps</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1445-L1490)

```python
sweeps()
```




<h3 id="to_html"><code>to_html</code></h3>

[View source](https://www.github.com/wandb/client/tree/latest/wandb/apis/public.py#L1425-L1433)

```python
to_html(
    height=420, hidden=(False)
)
```

Generate HTML containing an iframe displaying this project




