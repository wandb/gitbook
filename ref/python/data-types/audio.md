# wandb.data\_types.Audio

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/v0.10.33/wandb/data_types.py#L907-L1051)

Wandb class for audio clips.

```python
Audio(
    data_or_path, sample_rate=None, caption=None
)
```

| Arguments |  |
| :--- | :--- |
| `data_or_path` | \(string or numpy array\) A path to an audio file or a numpy array of audio data. |
| `sample_rate` | \(int\) Sample rate, required when passing in raw numpy array of audio data. |
| `caption` | \(string\) Caption to display with audio. |

## Methods

### `durations` <a id="durations"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/data_types.py#L1009-L1011)

```python
@classmethod
durations(
    audio_list
)
```

### `path_is_reference` <a id="path_is_reference"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/data_types.py#L952-L954)

```python
@classmethod
path_is_reference(
    path
)
```

### `resolve_ref` <a id="resolve_ref"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/data_types.py#L1025-L1037)

```python
resolve_ref()
```

### `sample_rates` <a id="sample_rates"></a>

[View source](https://www.github.com/wandb/client/tree/v0.10.33/wandb/data_types.py#L1013-L1015)

```python
@classmethod
sample_rates(
    audio_list
)
```

