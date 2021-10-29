# Project



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L1166-L1201)



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

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L1199-L1201)

```python
artifacts_types(
    per_page=50
)
```




<h3 id="display"><code>display</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L736-L747)

```python
display(
    height=420, hidden=(False)
) -> bool
```

Display this object in jupyter


<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L732-L734)

```python
snake_to_camel(
    string
)
```




<h3 id="to_html"><code>to_html</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L1183-L1191)

```python
to_html(
    height=420, hidden=(False)
)
```

Generate HTML containing an iframe displaying this project




