---
description: Appropriate limits and guidelines for logging data to Weights & Biases
---

# Limits & Performance

## Best Practices for Fast Pages

Keep your pages in W\&B faster and more responsive by logging within these suggested bounds.

### Logged Metrics

Use `wandb.log` to track experiment metrics. Once logged, these metrics generate charts and show up in tables. Too much logged data can make the app slow.

#### **Count of Distinct Metrics**

Keep the total number of distinct metrics under 10,000.

```python
wandb.log({
  "a": 1, # "a" is a distinct metric
  "b": {
    "c": "hello",  # "b.c" is a distinct metric
    "d": [1, 2, 3] # "b.d" is a distinct metric
}) # 3 distinct metrics logged
```

{% hint style="warning" %}
We automatically flatten nested values, so if you pass us a dictionary we will turn it into a dot-separated name. For config values, we support 3 dots in the name. For summary values, we support 4 dots.
{% endhint %}

Log related media to the same metric name:

```python
for i, img in enumerate(images):
  # ❌ not recommended
  wandb.log({
   f"pred_img_{i}": wandb.Image(image)
  })
  
 # ✅ recommended
 wandb.log({
   "pred_imgs": [wandb.Image(image) for image in images]
 }) 
```

Logging beyond 10,000 distinct metrics can slow down your project workspaces and runs table operations.

#### Value Width

Limit the size of a single logged value to under 1 MB and the total size of a single `wandb.log` call to under 25 MB. This limit does not apply to `wandb.Media` types like `wandb.Image`, `wandb.Audio`, etc.

```python
# ❌ not recommended
wandb.log({
  "wide_key": range(10000000)
})

# ❌ not recommended
with f as open('large_file.json', 'r'):
  large_data = json.load(f)
  wandb.log(large_data) 
```

If you log values wider than these recommendations your data will be saved and tracked, but your plots may load more slowly. Note that wide values can affect the plot load times for all metrics in the run, not just the metric with the wide values.

#### Metric Frequency

Pick a logging frequency that is appropriate to the metric you are logging. As a general rule of thumb, the wider the metric the less frequently you should log it. Concretely, we recommend:

* **Scalars**: <100,000 logged points per metric
* **Media**: <50,000 logged points per metric &#x20;
* **Histograms**: <10,000 logged points per metric

{% hint style="warning" %}
Plots in the W\&B UI downsample to 1,500 points per metric. Use the [Public API](public-api-guide.md) to access your unsampled data.
{% endhint %}

```python
# Training loop with 1m total steps
for step in range(1000000):
  # ❌ not recommended
  wandb.log({
  'scalar': step, # 100,000 scalars
  'media': wandb.Image(...), # 100,000 images
  'histogram': wandb.Histogram(...) # 100,000 histograms
  })
  
  # ✅ recommended
  if step % 1000 == 0:
    wandb.log({
      'histogram': wandb.Histogram(...), # 10,000 histograms
    }, commit=False)
  if step % 200 == 0:
    wandb.log({
      'media': wandb.Image(...), # 50,000 images
    }, commit=False)
  if step % 100 == 0:
    wandb.log({
      'scalar': step, # 100,000 scalars
    }, commit=True) # Commit batched, per-step metrics together
```

Enable batching in calls to `wandb.log` by passing `commit=False` to minimize the total number of API calls for a given step. See [the docs](../../ref/python/log.md) for `wandb.log` for more details.

If you exceed these guidelines, W\&B will continue to accept your logged data but pages may load more slowly.

#### Config Size

Limit the total size of your run config to <10MB. Logging large values could slow down your project workspaces and runs table operations.

```python
# ✅ recommended
wandb.init(config={
  "lr": 0.1,
  "batch_size": 32,
  "epocs": 4,
})

# ❌ not recommended
wandb.init(config={
  "steps": range(10000000),
})
 
# ❌ not recommended
with f as open('large_config.json', 'r'):
  large_config = json.load(f)
  wandb.init(config=large_config)
```

#### Run Count

Keep the total number of runs in a single project under 10,000. Large run counts can slow down project workspaces and runs table operations, especially when grouping is enabled or runs have a large count of distinct metrics.

#### File Count

Keep the total number of files uploaded for a single file under 1,000. You can use [W\&B Artifacts](../artifacts/api.md) when you need to log a large number of files. Exceeding 1,000 files in a single run can slow down your run pages.

### Python Script Performance

There are few common ways the performance of your python script can be reduced:

1. The size of your data is too large. Large data sizes could introduce a >1ms overhead to the training loop.
2. The speed of your network and the how the W\&B backend is configured
3. Calling `wandb.log` more than a few times per second. This is due to a small latency added to the training loop every time `wandb.log` is called.

{% hint style="info" %}
Is frequent logging slowing your training runs down? Check out [this Colab](http://wandb.me/log-hf-colab) for methods to get better performance by changing your logging strategy.
{% endhint %}

We do not assert any limits beyond rate limiting. Our Python client will automatically do an exponential backoff and retry requests that exceed limits, so this should be transparent to you. It will say “Network failure” on the command line. For unpaid accounts, we may reach out in extreme cases where usage exceeds reasonable thresholds.

### Rate Limits

The W\&B API is rate limited by IP and API key. New accounts are restricted to 200 requests per minute. This rate allows you to run approximately 15 processes in parallel and have them report without being throttled. If the **wandb** client detects it's being limited, it will backoff and retry sending the data in the future. If you need to run more than 15 processes in parallel send an email to [contact@wandb.com](mailto:contact@wandb.com).

For sweeps, we support up to 20 parallel agents.
