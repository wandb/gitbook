# Metaflow

## Overview

Metaflow is a framework created by Netflix for creating and running ML workflows.

This integration lets users apply decorators to Metaflow `Flow` and `Step` to automatically log parameters and artifacts to W&B by typedispatch.

* Decorating a Step will enable or disable logging for certain types within that Step.
* Decorating the Flow is equivalent to decorating all Steps with a default

## Quickstart

### Install W&B

{% tabs %}
{% tab title="Notebook" %}
```python
!pip install -Uqq wandb

import wandb
wandb.login()
```
{% endtab %}

{% tab title="Command Line" %}
```
pip install wandb
wandb login
```
{% endtab %}
{% endtabs %}

### Decorate your Flow and Steps

{% tabs %}
{% tab title="Step" %}
Decorating a **Step** will enable or disable logging for certain types within that Step.

* In this case, all datasets and models in `start` will be logged

```python
from wandb.integration.metaflow import wandb_log

class WandbExampleFlow(FlowSpec):
    @wandb_log(datasets=True, models=True, settings=wandb.Settings(...))
    @step
    def start(self):
        self.raw_df = pd.read_csv(...).    # pd.DataFrame -> upload as dataset
        self.model_file = torch.load(...)  # nn.Module    -> upload as model
        self.next(self.transform)
```
{% endtab %}

{% tab title="Flow" %}
Decorating the **Flow** is equivalent to decorating all Steps with a default.

* In this case, all steps in `WandbExampleFlow` will log datasets and models by default.
* This is the same as decorating each step with `@wandb_log(datasets=True, models=True)`

```python
from wandb.integration.metaflow import wandb_log

@wandb_log(datasets=True, models=True)  # decorate all @step 
class WandbExampleFlow(FlowSpec):
    @step
    def start(self):
        self.raw_df = pd.read_csv(...).    # pd.DataFrame -> upload as dataset
        self.model_file = torch.load(...)  # nn.Module    -> upload as model
        self.next(self.transform)
```
{% endtab %}

{% tab title="Both Flow and Steps" %}
Decorating the **Flow** is equivalent to decorating all Steps with a default.

* If you later decorate a **Step** with another `@wandb_log`, you will override the Flow decoration
* In this case:
  * `start` and `mid` will log datasets and models by default, but
  * `end` will NOT log datasets OR models

```python
from wandb.integration.metaflow import wandb_log

@wandb_log(datasets=True, models=True)  # same as decorating start and mid
class WandbExampleFlow(FlowSpec):
    # This step will log datasets and models
    @step
    def start(self):
        self.raw_df = pd.read_csv(...).    # pd.DataFrame -> upload as dataset
        self.model_file = torch.load(...)  # nn.Module    -> upload as model
        self.next(self.mid)

    # This step will also log datasets and models
    @step
    def mid(self):
        self.raw_df = pd.read_csv(...).    # pd.DataFrame -> upload as dataset
        self.model_file = torch.load(...)  # nn.Module    -> upload as model
        self.next(self.end)

    # This step is overwritten and will NOT log datasets OR models
    @wandb_log(datasets=False, models=False)
    @step
    def end(self):
        self.raw_df = pd.read_csv(...).    
        self.model_file = torch.load(...)
```
{% endtab %}
{% endtabs %}

## Where is my data?

| Data | CLI | UI |
| :--- | :--- | :--- |
| `Parameter(...)` | `wandb.config` | Overview tab &gt;&gt; Config  |
| `datasets, models, others` | `wandb.use_artifact("{var_name}:latest")` | Artifacts tab |
| Base python types \(dict, list, set, str, int, float, bool\) | `wandb.summary` | Overview tab &gt;&gt; Summary |

* `Parameter` are saved to W&B's config dict and can be found in the Overview tab
* `datasets`, `models`, and `others` are saved to W&B Artifacts and can be found in the Artifacts tab
* Base python types are saved to W&B's summary dict and can be found in the Overview tab.

## Advanced

### What gets logged?

#### Instance vs. Local variables

`wandb_log` only logs instance variables.  Local variables are NEVER logged.  This is useful to avoid logging unnecessary data.  

#### Type-based logging

We currently support these types:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Logging Setting</th>
      <th style="text-align:left">Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">default (always on)</td>
      <td style="text-align:left">
        <ul>
          <li><code>dict, list, set, str, int, float, bool</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>datasets</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li><code>pd.DataFrame</code>
          </li>
          <li><code>pathlib.Path</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>models</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li><code>nn.Module</code>
          </li>
          <li><code>sklearn.base.BaseEstimator</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>others</code>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Anything that is pickle-able</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

#### Examples of logging behaviour

| Kind of Variable | Behaviour | Example | Data Type |
| :--- | :--- | :--- | :--- |
| Instance | Auto- logged | `self.accuracy` | `float` |
| Instance | Auto-logged \(if `datasets=True`\) | `self.df` | `pd.DataFrame` |
| Instance | Not logged \(if `datasets=False`\) | `self.df` | `pd.DataFrame` |
| Local | Never logged | `accuracy` | `float` |
| Local | Never logged | `df` | `pd.DataFrame` |

### `wandb_log` kwargs

<table>
  <thead>
    <tr>
      <th style="text-align:left">kwarg</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">datasets</td>
      <td style="text-align:left">
        <ul>
          <li><code>True</code>: Log instance variables that are a dataset</li>
          <li><code>False</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">models</td>
      <td style="text-align:left">
        <ul>
          <li><code>True</code>: Log instance variables that are a model</li>
          <li><code>False</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">others</td>
      <td style="text-align:left">
        <ul>
          <li><code>True</code>: Log anything else that is serializable as a pickle</li>
          <li><code>False</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">settings</td>
      <td style="text-align:left">
        <ul>
          <li><code>wandb.Settings(...)</code>: Specify your own wandb settings for
            this step(s)</li>
          <li><code>None</code>: Equivalent to passing <code>wandb.Settings()</code>
          </li>
        </ul>
        <p>By default, if:</p>
        <ul>
          <li><code>settings.run_group</code> is None, it will be set to <code>{flow_name}/{run_id}</code>
          </li>
          <li><code>settings.run_job_type</code> is None, it will be set to <code>{run_job_type}/{step_name}</code>
          </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>



