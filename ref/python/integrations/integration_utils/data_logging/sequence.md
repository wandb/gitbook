# Sequence







All the operations on a read-only sequence.


Concrete subclasses must override __new__ or __init__,
__getitem__, and __len__.

## Methods

<h3 id="count"><code>count</code></h3>

```python
count(
    value
)
```

S.count(value) -> integer -- return number of occurrences of value


<h3 id="index"><code>index</code></h3>

```python
index(
    value, start=0, stop=None
)
```

S.index(value, [start, [stop]]) -> integer -- return first index of value.
Raises ValueError if the value is not present.

Supporting start and stop arguments is optional, but
recommended.

<h3 id="__contains__"><code>__contains__</code></h3>

```python
__contains__(
    value
)
```




<h3 id="__getitem__"><code>__getitem__</code></h3>

```python
__getitem__(
    index
)
```




<h3 id="__iter__"><code>__iter__</code></h3>

```python
__iter__()
```




<h3 id="__len__"><code>__len__</code></h3>

```python
__len__()
```






