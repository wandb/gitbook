# Table

[![https://www.tensorflow.org/images/GitHub-Mark-32px.png](https://lh3.googleusercontent.com/EO5eqAPA5mPxwqDYuiMcVehc8mQskljktlxRa-SGYJ1xIOKVpKXJDW2Qh-1xWpYJW-2XijZurbSrt58xOJCQOt1miCkeDIOImPckmS_g17XIOgeX34Mcxnt5CdiP93ThHBKRUJfL1b4RC90_NA)GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L481-L770)**​**​

일련의 기록을 표시하도록 설계된 테이블입니다.

```text
Table(    columns=None, data=None, rows=None, dataframe=None, dtype=None, optional=True,    allow_mixed_types=False)
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#xC804;&#xB2EC;&#xC778;&#xC790;</b>
      </th>
      <th style="text-align:left">&#x200B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"> <code>columns</code>
      </td>
      <td style="text-align:left">([str]) &#xD14C;&#xC774;&#xBE14;&#xC758; &#xC5F4; &#xC774;&#xB984;&#xC785;&#xB2C8;&#xB2E4;.
        &#xAE30;&#xBCF8;&#xAC12;&#xC740; [&quot;Input&quot;, &quot;Output&quot;,
        &quot;Expected&quot;]&#xC785;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <code>data</code>
      </td>
      <td style="text-align:left">(array) &#xC2A4;&#xD2B8;&#xB9C1;(string)&#xC73C;&#xB85C; &#xD45C;&#xC2DC;&#xB420;
        &#xAC12;&#xC758; 2&#xCC28;&#xC6D0; &#xBC30;&#xC5F4;&#xC785;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <code>dataframe</code>
      </td>
      <td style="text-align:left">
        <p>(pandas.DataFrame) Dataframe &#xAC1D;&#xCCB4;&#xB294; &#xD14C;&#xC774;&#xBE14;&#xC740;
          &#xC0DD;&#xC131;&#xD558;&#xB294;&#xB370; &#xC0AC;&#xC6A9;&#xB429;&#xB2C8;&#xB2E4;.
          &#xC124;&#xC815;&#xC774; &#xB418;&#xBA74;, &#xB2E4;&#xB978; &#xC804;&#xB2EC;&#xC778;&#xC790;&#xB294;
          &#xBB34;&#xC2DC;&#xB429;&#xB2C8;&#xB2E4;.</p>
        <p>optional (Union[bool,List[bool]]): None&#xC778; &#xACBD;&#xC6B0; &#xAC12;&#xC774;
          &#xD5C8;&#xC6A9;&#xB429;&#xB2C8;&#xB2E4;. &#xB2E8;&#xC77C; &#xBD88;(bool)&#xC774;
          &#xBAA8;&#xB4E0; &#xC5F4;&#xC5D0; &#xC801;&#xC6A9;&#xB429;&#xB2C8;&#xB2E4;.</p>
        <p>allow_mixed_types (bool): &#xC5F4;&#xC5D0; &#xD63C;&#xD569; &#xD615;&#xC2DD;&#xC744;
          &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB294;&#xC9C0; &#xC5EC;&#xBD80;&#xB97C;
          &#xACB0;&#xC815;&#xD569;&#xB2C8;&#xB2E4; (&#xC720;&#xD615; &#xAC80;&#xC0AC;
          &#xBE44;&#xD65C;&#xC131;&#xD654;). &#xAE30;&#xBCF8;&#xAC12;&#xC740; False&#xC785;&#xB2C8;&#xB2E4;.</p>
      </td>
    </tr>
  </tbody>
</table>

## **방법** <a id="methods"></a>

### `add_data` <a id="add_data"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L636-L645)**​**​

```text
add_data(    *data)
```

데이터 행을 테이블에 추가합니다. 전달인자 길이는 열 길이와 일치해야 합니다.

### `add_row` <a id="add_row"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L632-L634)**​**​

```text
add_row(    *row)
```

### `cast` <a id="cast"></a>

​[소스 보기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L594-L611)**​**​

```text
cast(    col_name, dtype, optional=False)
```

### `iterrows` <a id="iterrows"></a>

​ [소스 보기](https://www.github.com/wandb/client/tree/master/wandb/data_types.py#L760-L770)**​**​

```text
iterrows()
```

여러 행에 걸처 \(ndx, row\)로 반복합니다

## **Yields\(산출\)** <a id="yields"></a>

index: int 행 인덱스. row: List\[any\] 열 데이터

| **클래스 변수** | ​ |
| :--- | :--- |
|  MAX\_ARTIFACT\_ROWS |  \`200000\` |
|  MAX\_ROWS |  \`10000\` |
|  artifact\_type |  \`'table'\` |

