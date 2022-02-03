# file

## Chainable Ops
<h3 id="file-contents"><code>file-contents</code></h3>

Returns the contents of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The contents of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="file-digest"><code>file-digest</code></h3>

Returns the digest of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The digest of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="file-size"><code>file-size</code></h3>

Returns the size of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The size of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="file-table"><code>file-table</code></h3>

Returns the contents of the [file](https://docs.wandb.ai/ref/weave/types/file) as a _table_

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The contents of the [file](https://docs.wandb.ai/ref/weave/types/file) as a _table_


## List Ops
<h3 id="file-contents"><code>file-contents</code></h3>

Returns the contents of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The contents of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="count"><code>count</code></h3>

Returns the count of elements in the _list_.

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to count. |

#### Return Value
The count of elements in the _list_.

<h3 id="file-digest"><code>file-digest</code></h3>

Returns the digest of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The digest of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="dropna"><code>dropna</code></h3>

Drops elements of a _list_ which are null

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to drop elements from. |

#### Return Value
The _list_ with null elements removed.

<h3 id="filter"><code>filter</code></h3>

Filters the _list_.

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to filter. |
| `filterFn` | A function to apply to each element of the _list_. The return value is a boolean indicating whether the element should be included in the result. |

#### Return Value
The filtered _list_.

<h3 id="joinToStr"><code>joinToStr</code></h3>

Joins the elements of the _list_ into a _string_.

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to join. |
| `sep` | The separator to use between elements. |

#### Return Value
The joined _string_.

<h3 id="map"><code>map</code></h3>

Applies a map function to each element in the _list_

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to map over. |
| `mapFn` | A function to apply to each element of the _list_. |

#### Return Value
The _list_ with each element mapped over.

<h3 id="file-size"><code>file-size</code></h3>

Returns the size of the [file](https://docs.wandb.ai/ref/weave/types/file)

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The size of the [file](https://docs.wandb.ai/ref/weave/types/file)

<h3 id="file-table"><code>file-table</code></h3>

Returns the contents of the [file](https://docs.wandb.ai/ref/weave/types/file) as a _table_

| Argument |  | 
| :--- | :--- |
| `file` | A [file](https://docs.wandb.ai/ref/weave/types/file) |

#### Return Value
The contents of the [file](https://docs.wandb.ai/ref/weave/types/file) as a _table_

<h3 id="index"><code>index</code></h3>

Retrieve a value from a _list_ by index

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to index into. |
| `index` | The index to retrieve |

#### Return Value
A value from the _list_

