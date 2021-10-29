# Sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L1973-L2143)



A set of runs associated with a sweep.

```python
Sweep(
    client, entity, project, sweep_id, attrs={}
)
```





#### Examples:

Instantiate with:
```
api = wandb.Api()
sweep = api.sweep(path/to/sweep)
```





| Attributes |  |
| :--- | :--- |
|  `runs` |  (`Runs`) list of runs |
|  `id` |  (str) sweep id |
|  `project` |  (str) name of project |
|  `config` |  (str) dictionary of sweep configuration |
|  `state` |  (str) the state of the sweep |



## Methods

<h3 id="best_run"><code>best_run</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L2049-L2072)

```python
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in


<h3 id="display"><code>display</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L736-L747)

```python
display(
    height=420, hidden=(False)
) -> bool
```

Display this object in jupyter


<h3 id="get"><code>get</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L2088-L2127)

```python
@classmethod
get(
    client, entity=None, project=None, sid=None, order=None, query=None, **kwargs
)
```

Execute a query against the cloud backend


<h3 id="load"><code>load</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L2030-L2038)

```python
load(
    force=(False)
)
```




<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L732-L734)

```python
snake_to_camel(
    string
)
```




<h3 id="to_html"><code>to_html</code></h3>

[View source](https://www.github.com/wandb/client/tree/a71719bdde474b8048d942c5b1be20afadaef59a/wandb/apis/public.py#L2129-L2137)

```python
to_html(
    height=420, hidden=(False)
)
```

Generate HTML containing an iframe displaying this sweep






| Class Variables |  |
| :--- | :--- |
|  `QUERY`<a id="QUERY"></a> |   |

