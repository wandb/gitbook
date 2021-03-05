---
description: wandb.apis.public
---

# File

​[​![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh3.googleusercontent.com/mZx3DCi76qt4qf2er7aAi9ZEn6jjf37pnH2F-plgc6oKu4DQxqA4I-uOThRFvdOxwQgjoUTCBRfXpmGDQi_npbPIi30wT1AzJGSOPwDxejlbTDkajmgSH8yNR_WZ3rklpadzV0nli5tJhmVpQQ)GitHub](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1603-L1704)​에서 소스 확인하기

File은 wandb가 저장하는 파일과 관련된 클래스입니다.

```text
File(
    client, attrs
)
```

| **속성** |  |
| :--- | :--- |
| `digest` |  |
| `direct_url` |  |
| `id` |  |
| `md5` |  |
| `mimetype` |  |
| `name` |  |
| `size` |  |
| `updated_at` |  |
| `url` |  |

### **방법**

### `delete` <a id="delete"></a>

[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1686-L1699)**​**

```text
delete()
```

### `download` <a id="download"></a>

 [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1663-L1684)

```text
download(
    root='.', replace=False
)
```

wandb 서버에서 실행에 의해 이전에 저장된 파일을 다운로드합니다.

| **전달인자** |
| :--- |
| replace \(boolean\): \`True\`이면 로컬 파일이 존재하는 경우 download가 로컬 파일을 덮어씁니다. 기본값은 \`False\`입니다. |

| **발생\(Raises\)** |
| :--- |
| \`ValueError\` 이미 파일이 존재하고 replace=False인 경우 |

