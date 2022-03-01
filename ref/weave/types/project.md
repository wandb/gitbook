# project

## Chainable Ops
<h3 id="project-artifactType"><code>project-artifactType</code></h3>

Returns the _artifactType_ for a given name within a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |
| `artifactType` | The name of the _artifactType_ |

#### Return Value
The _artifactType_ for a given name within a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-artifactTypes"><code>project-artifactTypes</code></h3>

Returns the _artifactTypes_ for a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The _artifactTypes_ for a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-artifactVersion"><code>project-artifactVersion</code></h3>

Returns the _artifactVersion_ for a given name and version within a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |
| `artifactName` | The name of the _artifactVersion_ |
| `artifactVersionAlias` | The version alias of the _artifactVersion_ |

#### Return Value
The _artifactVersion_ for a given name and version within a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-createdAt"><code>project-createdAt</code></h3>

Returns the creation time of the [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The creation time of the [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="isNone"><code>isNone</code></h3>

Determines if the value is None.

| Argument |  | 
| :--- | :--- |
| `val` | Possibly None value. |

#### Return Value
True if the value is None.

<h3 id="project-name"><code>project-name</code></h3>

Returns the name of the [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The name of the [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-runs"><code>project-runs</code></h3>

Returns the [runs](https://docs.wandb.ai/ref/weave/types/run) from a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The [runs](https://docs.wandb.ai/ref/weave/types/run) from a [project](https://docs.wandb.ai/ref/weave/types/project)


## List Ops
<h3 id="project-artifactType"><code>project-artifactType</code></h3>

Returns the _artifactType_ for a given name within a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |
| `artifactType` | The name of the _artifactType_ |

#### Return Value
The _artifactType_ for a given name within a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-artifactTypes"><code>project-artifactTypes</code></h3>

Returns the _artifactTypes_ for a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The _artifactTypes_ for a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-artifactVersion"><code>project-artifactVersion</code></h3>

Returns the _artifactVersion_ for a given name and version within a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |
| `artifactName` | The name of the _artifactVersion_ |
| `artifactVersionAlias` | The version alias of the _artifactVersion_ |

#### Return Value
The _artifactVersion_ for a given name and version within a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="count"><code>count</code></h3>

Returns the count of elements in the _list_.

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to count. |

#### Return Value
The count of elements in the _list_.

<h3 id="project-createdAt"><code>project-createdAt</code></h3>

Returns the creation time of the [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The creation time of the [project](https://docs.wandb.ai/ref/weave/types/project)

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

<h3 id="isNone"><code>isNone</code></h3>

Determines if the value is None.

| Argument |  | 
| :--- | :--- |
| `val` | Possibly None value. |

#### Return Value
True if the value is None.

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

<h3 id="project-name"><code>project-name</code></h3>

Returns the name of the [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The name of the [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="project-runs"><code>project-runs</code></h3>

Returns the [runs](https://docs.wandb.ai/ref/weave/types/run) from a [project](https://docs.wandb.ai/ref/weave/types/project)

| Argument |  | 
| :--- | :--- |
| `project` | A [project](https://docs.wandb.ai/ref/weave/types/project) |

#### Return Value
The [runs](https://docs.wandb.ai/ref/weave/types/run) from a [project](https://docs.wandb.ai/ref/weave/types/project)

<h3 id="index"><code>index</code></h3>

Retrieve a value from a _list_ by index

| Argument |  | 
| :--- | :--- |
| `arr` | The _list_ to index into. |
| `index` | The index to retrieve |

#### Return Value
A value from the _list_

