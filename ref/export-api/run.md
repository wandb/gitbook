# Run

  
​​[![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh4.googleusercontent.com/qg-vrxAAWVUCnmiq3GlvOF59psOWCx5L8Wau2FlP7GwdSDWVxazOj1VtXI-tvsYVDVaZqzKrlWcmFjeGBBB91HoGaFDqScRG_HTKVz14Q3lVE2eEV-k7KH9Xaw8gbgBraiS87v4wq1tTCc4HVQ)GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L810-L1353)**​**​

개체\(entity\) 및 프로젝트와 관련된 단일 실행\(run\)

```text
Run(    client, entity, project, run_id, attrs={})
```

| **속성** | ​ |
| :--- | :--- |
| `entity` | ​ |
| `id` | ​ |
| `json_config` | ​ |
| `lastHistoryStep` | ​ |
| `name` | ​ |
| `path` | ​ |
| `storage_id` | ​ |
| `summary` | ​ |
| `url` | ​ |
| `username` | ​ |

## **방법** <a id="methods"></a>

### `create` <a id="create"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L892-L932)**​**​

```text
@classmethodcreate(    api, run_id=None, project=None, entity=None)
```

지정된 프로젝트에 대한 실행을 생성합니다.

### `delete` <a id="delete"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1027-L1061)**​**​

```text
delete(    delete_artifacts=False)
```

wandb 백엔드에서 주어진 실행을 삭제합니다.

### `file` <a id="file"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1111-L1121)**​**​

```text
file(    name)
```

전달인자: names \(list\): 요청된 files의 이름으로, 비어 있으면 모든 files를 반환합니다.

| **반환** |
| :--- |
| \`Files\` 객체, 즉 \`File\` 객체를 반복하는 반복자\(iterator\)입니다. |

### `files` <a id="files"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1157-L1189)**​**​

```text
files(    names=[], per_page=50)
```

 실행에 대한 샘플링된 히스토리 메트릭을 반환합니다. 히스토리 기록이 샘플링되는 것이 괜찮으시다면 이 작업이 더 간단하고 빠릅니다.

| **전달인자** |
| :--- |
| `Files` 객체, 즉 `File` 객체를 반복하는 반복자\(iterator\)입니다. |

### `history` <a id="history"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1157-L1189)​​

```text
history(    samples=500, keys=None, x_axis='_step', pandas=True,    stream='default')
```

실행에 대한 샘플링된 히스토리 메트릭을 반환합니다. 히스토리 기록이 샘플링되는 것이 괜찮으시다면 이 작업이 더 간단하고 빠릅니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>samples (int, optional): &#xBC18;&#xD658;&#xD560; &#xC0D8;&#xD50C; &#xC218;</p>
        <p>pandas (bool, optional): panda dataframe(&#xD310;&#xB2E4; &#xB370;&#xC774;&#xD130;&#xD504;&#xB808;&#xC784;)&#xC744;
          &#xBC18;&#xD658;&#xD569;&#xB2C8;&#xB2E4;.</p>
        <p>keys (list, optional): &#xD2B9;&#xC815; &#xD0A4;(keys)&#xC5D0; &#xB300;&#xD55C;
          &#xBA54;&#xD2B8;&#xB9AD;&#xB9CC; &#xBC18;&#xD658;&#xD569;&#xB2C8;&#xB2E4;.</p>
        <p>x_axis (str, optional): &#xC774; &#xBA54;&#xD2B8;&#xB9AD;&#xC744; xAxis&#xB85C;
          &#xC0AC;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;. &#xAE30;&#xBCF8;&#xAC12;&#xC740;
          _step stream (str, optional): &#xBA54;&#xD2B8;&#xB9AD;&#xC758; &#xACBD;&#xC6B0;&#x201C;default&#x201D;,
          &#xBA38;&#xC2E0; &#xBA54;&#xD2B8;&#xB9AD;&#xC758; &#xACBD;&#xC6B0; &quot;system&quot;&#xC785;&#xB2C8;&#xB2E4;.</p>
      </td>
    </tr>
  </tbody>
</table>

|  반환 |
| :--- |
| pandas=True인 경우 히스토릭 메트릭의 `pandas.DataFrame`를 반환합니다. pandas=False인 경우, 히스토리 메트릭의 dict의 리스트를 반환합니다. |

### `load` <a id="load"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L934-L996)​​

```text
load(    force=False)
```

### `log_artifact` <a id="log_artifact"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1275-L1307)​​

```text
log_artifact(    artifact, aliases=None)
```

아티팩트를 실행의 출력으로 선언\(declare\)합니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>artifact (`Artifact`): `wandb.Api().artifact(name)`&#xC5D0;&#xC11C; &#xBC18;&#xD658;&#xB41C;
          &#xC544;&#xD2F0;&#xD329;&#xD2B8;</p>
        <p>aliases (list, optional): &#xC774; &#xC544;&#xD2F0;&#xD329;&#xD2B8;&#xC5D0;
          &#xC801;&#xC6A9;&#xD560; aliases(&#xBCC4;&#xCE6D;)</p>
      </td>
    </tr>
  </tbody>
</table>

| **전달인자** |
| :--- |
| \`Artifact\` 객체 |

### `logged_artifacts` <a id="logged_artifacts"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1240-L1242)​

```text
logged_artifacts(    per_page=100)
```

### `save` <a id="save"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1063-L1064)**​**​

```text
save()
```

### `scan_history` <a id="scan_history"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1191-L1238)**​**​

```text
scan_history(    keys=None, page_size=1000, min_step=None, max_step=None)
```

실행에 대한 반복 가능한 모든 히스토리 기록 모음을 반환합니다.

#### **예시:** <a id="example"></a>

예시 실행에 대한 모든 손실 값은 내보냅니다.  


```text
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")history = run.scan_history(keys=["Loss"])losses = [row["Loss"] for row in history]
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>keys ([str], optional): &#xC774;&#xB7EC;&#xD55C; &#xD0A4;&#xB9CC;&#xC744;
          &#xAC00;&#xC838;&#xC624;&#xBA70;, &#xBAA8;&#xB4E0; &#xD0A4;&#xAC00; &#xC815;&#xC758;&#xB41C;
          &#xD589;&#xB9CC; &#xAC00;&#xC838;&#xC635;&#xB2C8;&#xB2E4;.</p>
        <p>page_size (int, optional): api&#xC5D0;&#xC11C; &#xAC00;&#xC838;&#xC62C;
          &#xD398;&#xC774;&#xC9C0; &#xD06C;&#xAE30;</p>
      </td>
    </tr>
  </tbody>
</table>

| **반환** |
| :--- |
| 히스토리 기록\(dict\)에 대한 반복 가능한 모음\(collection\) |

### `snake_to_camel` <a id="snake_to_camel"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L528-L530)**​**​

```text
snake_to_camel(    string)
```

### `update` <a id="update"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L998-L1025)**​**​

```text
update()
```

실행 객체에 대한 변경 사항을 wandb 백엔드에 계속 유지합니다.  


### `upload_file` <a id="upload_file"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1134-L1155)**​**​

```text
upload_file(    path, root='.')
```

root \(str\): 관련된 파일을 저장할 루트\(root\) 경로. 즉, 실행에서 파일을 "my\_dir/file.txt"로 저장하고자 하고 현재 "my\_dir"에 있는 경우 루트\(root\)를 "../"로 설정합니다.  


| 반환 |
| :--- |
| name 전달인자와 일치하는 \`File\` |

### `use_artifact` <a id="use_artifact"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1248-L1273)**​**​

```text
use_artifact(    artifact)
```

아티팩트를 실행에 대한 입력으로 선언\(declare\)합니다.

| 전달인자 |
| :--- |
| artifact \(`Artifact`\): `wandb.Api().artifact(name)`에서 반환된 아티팩트 |

| **반환** |
| :--- |
| \`Artifact\` 객체 |

### `used_artifacts` <a id="used_artifacts"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/apis/public.py#L1244-L1246)**​**​

```text
used_artifacts(    per_page=100)
```

