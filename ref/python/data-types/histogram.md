# wandb.data\_types.Histogram

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.28/wandb/sdk/data_types.py#L265-L342)

wandb class for histograms.

```text
Histogram(
    sequence: Optional[Sequence] = None,
    np_histogram: Optional['NumpyHistogram'] = None,
    num_bins: int = 64
) -> None
```

This object works just like numpy's histogram function [https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)

## Examples:

Generate histogram from a sequence

```python
wandb.Histogram([1,2,3])
```

Efficiently initialize from np.histogram.

```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```

| Arguments |  |
| :--- | :--- |
|  `sequence` |  \(array\_like\) input data for histogram |
|  `np_histogram` |  \(numpy histogram\) alternative input of a precoomputed histogram |
|  `num_bins` |  \(int\) Number of bins for the histogram. The default number of bins is 64. The maximum number of bins is 512 |

| Attributes |  |
| :--- | :--- |
|  `bins` |  \(\[float\]\) edges of bins |
|  `histogram` |  \(\[int\]\) number of elements falling in each bin |

| Class Variables |  |
| :--- | :--- |
|  MAX\_LENGTH |  `512` |

