# Histogram



[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.30/wandb/sdk/data_types.py#L265-L342)




wandb class for histograms.

<pre><code>Histogram(
    sequence: Optional[Sequence] = None,
    np_histogram: Optional['NumpyHistogram'] = None,
    num_bins: int = 64
) -> None</code></pre>




This object works just like numpy's histogram function
https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html

#### Examples:

Generate histogram from a sequence
```python
wandb.Histogram([1,2,3])
```

Efficiently initialize from np.histogram.
```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```



<!-- Tabular view -->
<table>
<tr><th>Arguments</th></tr>

<tr>
<td>
<code>sequence</code>
</td>
<td>
(array_like) input data for histogram
</td>
</tr><tr>
<td>
<code>np_histogram</code>
</td>
<td>
(numpy histogram) alternative input of a precoomputed histogram
</td>
</tr><tr>
<td>
<code>num_bins</code>
</td>
<td>
(int) Number of bins for the histogram.  The default number of bins
is 64.  The maximum number of bins is 512
</td>
</tr>
</table>





<!-- Tabular view -->
<table>
<tr><th>Attributes</th></tr>

<tr>
<td>
<code>bins</code>
</td>
<td>
([float]) edges of bins
</td>
</tr><tr>
<td>
<code>histogram</code>
</td>
<td>
([int]) number of elements falling in each bin
</td>
</tr>
</table>





<!-- Tabular view -->
<table>
<tr><th>Class Variables</th></tr>

<tr>
<td>
MAX_LENGTH<a id="MAX_LENGTH"></a>
</td>
<td>
<code>512</code>
</td>
</tr>
</table>

