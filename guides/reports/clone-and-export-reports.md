---
description: Clone and export reports into PDF or LaTeX.
---

# Clone and export reports

## Export reports

Export a report as a PDF or LaTeX. Within your report, select the kebab icon to expand the dropdown menu. Choose **Download and** select either PDF or LaTeX output format.&#x20;

## Cloning reports

{% tabs %}
{% tab title="App UI" %}
Within your report, select the kebab icon to expand the dropdown menu. Choose the **Clone this report** button. Pick a destination for your cloned report in the modal. Choose **Clone report**.

![](../../.gitbook/assets/clone\_reports.gif)

Clone a report to reuse a project's template and format. Cloned projects are visible to your team if you clone a project within the team's account. Projects cloned within an individual's account are only visible to that user.
{% endtab %}

{% tab title="Python SDK" %}
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/report\_api)

Load a Report from a URL to use it as a template.

<pre class="language-python"><code class="lang-python"><strong>report = wr.Report(
</strong>    project=PROJECT,
    title='Quickstart Report',
    description="That was easy!"
)                                              # Create
report.save()                                  # Save
<strong>new_report = wr.Report.from_url(report.url)    # Load
</strong></code></pre>

Edit the content within `new_report.blocks`.

```python
pg = wr.PanelGrid(
    runsets=[
        wr.Runset(ENTITY, PROJECT, "First Run Set"),
        wr.Runset(ENTITY, PROJECT, "Elephants Only!", query="elephant"),
    ],
    panels=[
        wr.LinePlot(x='Step', y=['val_acc'], smoothing_factor=0.8),
        wr.BarPlot(metrics=['acc']),
        wr.MediaBrowser(media_keys='img', num_columns=1),
        wr.RunComparer(diff_only='split', layout={'w': 24, 'h': 9}),
    ]
)

new_report.blocks = report.blocks[:1] + [wr.H1("Panel Grid Example"), pg] + report.blocks[1:]
new_report.save()
```
{% endtab %}
{% endtabs %}







