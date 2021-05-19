# Graph



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1247-L1407)




Wandb class for graphs

<pre><code>Graph(
    format=&#x27;keras&#x27;
)</code></pre>




This class is typically used for saving and diplaying neural net models.  It
represents the graph as an array of nodes and edges.  The nodes can have
labels that can be visualized by wandb.

#### Examples:

Import a keras model:
```
    Graph.from_keras(keras_model)
```



## Methods

<h3 id="add_edge"><code>add_edge</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1333-L1337">View source</a>

<pre><code>add_edge(
    from_node, to_node
)</code></pre>




<h3 id="add_node"><code>add_node</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1319-L1331">View source</a>

<pre><code>add_node(
    node=None, **node_kwargs
)</code></pre>




<h3 id="from_keras"><code>from_keras</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1339-L1368">View source</a>

<pre><code>@classmethod</code>
<code>from_keras(
    model
)</code></pre>




<h3 id="pprint"><code>pprint</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1313-L1317">View source</a>

<pre><code>pprint()</code></pre>




<h3 id="__getitem__"><code>__getitem__</code></h3>

<a target="_blank" href="https://www.github.com/wandb/client/tree/7e65d3b4f98261a70a14749af55a46433d6881c2/wandb/data_types.py#L1310-L1311">View source</a>

<pre><code>__getitem__(
    nid
)</code></pre>






