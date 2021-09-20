# Sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1497-L1654)



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

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1573-L1596)

```python
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in


<h3 id="get"><code>get</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1612-L1651)

```python
@classmethod
get(
    client, entity=None, project=None, sid=None, order=None, query=None, **kwargs
)
```

Execute a query against the cloud backend


<h3 id="load"><code>load</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L1554-L1562)

```python
load(
    force=(False)
)
```




<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.12.2/wandb/apis/public.py#L573-L575)

```python
snake_to_camel(
    string
)
```








| Class Variables |  |
| :--- | :--- |
|  `QUERY`<a id="QUERY"></a> |   |

