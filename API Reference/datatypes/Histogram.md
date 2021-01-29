# Histogram

<!-- Insert buttons and diff -->


[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L218-L283)




wandb class for histograms.

<pre class="devsite-click-to-copy prettyprint lang-py tfo-signature-link">
<code>datatypes.Histogram(
    sequence=None, np_histogram=None, num_bins=64
)
</code></pre>



<!-- Placeholder for "Used in" -->

This object works just like numpy's histogram function
https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html

#### Examples:

Generate histogram from a sequence
```
wandb.Histogram([1,2,3])
```

Efficiently initialize from np.histogram.
```
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
`512`
</td>
</tr><tr>
<td>
artifact_type<a id="artifact_type"></a>
</td>
<td>
`None`
</td>
</tr>
</table>

