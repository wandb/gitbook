# HTML

[![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh4.googleusercontent.com/vS-RYm86cd4ExUs3qKtLZjXvEDxpJ37IHd3vNqENilvCzBv7OmGsMLtYbibqG93T_S0OPpSdDRSXx-KHNAyNct2cmHGv82k4Wge3yy0ALggtf7gje5fED0BYs5GW6S-xVi_W9Jf9hpojMqaMZA)GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L1240-L1319)​

 임의의\(arbitrary\) html에 대한 Wandb 클래스

```text
Html(    data, inject=True)
```

| **전달인자** | ​ |
| :--- | :--- |
|  `data` | \(string 또는 io object\) wandb에 표시할 HTML |
|  `inject` |  \(boolean\) HTML 객체에 stylesheet\(스타일시트\)를 추가합니다. False로 설정된 경우 HTML은 변경되지 않은 상태\(unchanged\)로 통과합니다. |

## **방법** <a id="methods"></a>

### `inject_head` <a id="inject_head"></a>

​

```text
inject_head()
```

| **클래스 변수** | ​ |
| :--- | :--- |
|  artifact\_type |  \`'html-file'\` |

