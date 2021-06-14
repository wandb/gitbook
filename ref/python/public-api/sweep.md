# Sweep



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.32/wandb/apis/public.py#L1410-L1593)



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



## Methods

<h3 id="best_run"><code>best_run</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/apis/public.py#L1501-L1524)

```python
best_run(
    order=None
)
```

Returns the best run sorted by the metric defined in config or the order passed in


<h3 id="get"><code>get</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/apis/public.py#L1540-L1590)

```python
@classmethod
get(
    client, entity=None, project=None, sid=None, withRuns=(True), order=None,
    query=None, **kwargs
)
```

Execute a query against the cloud backend


<h3 id="load"><code>load</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/apis/public.py#L1481-L1490)

```python
load(
    force=(False)
)
```




<h3 id="snake_to_camel"><code>snake_to_camel</code></h3>

[View source](https://www.github.com/wandb/client/tree/v0.10.32/wandb/apis/public.py#L567-L569)

```python
snake_to_camel(
    string
)
```








| Class Variables |  |
| :--- | :--- |
|  `QUERY`<a id="QUERY"></a> |   |

