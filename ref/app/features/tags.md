# Tags

Tags can be used to label runs with particular features that might not be obvious from the logged metrics or Artifact data -- this run's model is `in_production`, that run is `preemptible`, this run represents the `baseline`.

## How to add tags <a href="how-to-add-tags" id="how-to-add-tags"></a>

You can add tags to a run when it is created: `wandb.init(tags=["tag1", "tag2"])` .

You can also update the tags of a run during training (e.g. if a particular metrics crosses a pre-defined threshold):

```python
run = wandb.init(entity="geoff", project="capsules", tags=["debug"])

...

if current_loss < threshold:
    run.tags = run.tags + ("release_candidate",)
```

There are also several ways to add tags after runs have been logged to Weights & Biases.

{% tabs %}
{% tab title="Using the Public API" %}
After a run is created, you can update tags using [our public API](../../../guides/track/public-api-guide.md) like so:

```python
run = wandb.Api().run("{entity}/{project}/{run-id}"})
run.tags.append("tag1")  # you can choose tags based on run data here
run.update()
```

You can read more about how to use the Public API in the [reference documentation](../../python/public-api/) or [guide](../../../guides/track/public-api-guide.md).
{% endtab %}

{% tab title="Project Page" %}
This method is best suited to tagging large numbers of runs with the same tag or tags.

In the [runs sidebar](../pages/project-page.md#search-for-runs) of the [Project Page](../pages/project-page.md),  click the table icon in the upper-right.  This will expand the sidebar into the full [runs table](runs-table.md).

Hover over a run in the table to see a checkbox on the left or look in the header row for a checkbox that will allow you to select all runs.

Click the checkbox to enable bulk actions. Select the runs to which you'd like to apply your tag(s).

Click the Tag button above the rows of runs.

Type a tag you'd like to add and click "Add" below the text box to add a new tag.
{% endtab %}

{% tab title="Run Page" %}
This method is best suited to applying a tag or tags to a single run by hand.\
\
In the left sidebar of the [Run Page](../pages/run-page.md), click the top [Overview tab](../pages/run-page.md#overview-tab).

Next to "Tags" is a gray ➕button. Click on that plus to add a tag.

Type a tag you'd like to add and click "Add" below the text box to add a new tag.
{% endtab %}
{% endtabs %}

## How to remove tags <a href="how-to-remove-tags" id="how-to-remove-tags"></a>

Tags can also be removed from runs via the UI.

{% tabs %}
{% tab title="Project Page" %}
This method is best suited to removing tags from a large numbers of runs.

In the [runs sidebar](../pages/project-page.md#search-for-runs) of the [Project Page](../pages/project-page.md),  click the table icon in the upper-right.  This will expand the sidebar into the full [runs table](runs-table.md).

Hover over a run in the table to see a checkbox on the left or look in the header row for a checkbox that will allow you to select all runs.

Click either checkbox to enable bulk actions. Select the runs to from which you'd like to remove your tag(s).

Click the Tag button above the rows of runs.

Click the checkbox next to a tag to remove it from the run.
{% endtab %}

{% tab title="Run Page" %}
In the left sidebar of the [Run Page,](../pages/run-page.md) click the top [Overview tab](../pages/run-page.md#overview-tab). The tags on the run are visible here.

Hover over a tag and click the "x" to remove it from the run.
{% endtab %}
{% endtabs %}
