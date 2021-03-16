# Audio

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)View source on GitHub](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L862-L976)

Wandb class for audio clips.

```text
Audio(
    data_or_path, sample_rate=None, caption=None
)
```

| Arguments |  |
| :--- | :--- |
|  `data_or_path` |  \(string or numpy array\) A path to an audio file or a numpy array of audio data. |
|  `sample_rate` |  \(int\) Sample rate, required when passing in raw numpy array of audio data. |
|  `caption` |  \(string\) Caption to display with audio. |

## Methods

### `durations` <a id="durations"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L952-L954)

```text
@classmethod
durations(
    audio_list
)
```

### `sample_rates` <a id="sample_rates"></a>

[View source](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L956-L958)

```text
@classmethod
sample_rates(
    audio_list
)
```

| Class Variables |  |
| :--- | :--- |
|  artifact\_type |  \`'audio-file'\` |

