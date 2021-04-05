# Alert

[![](https://www.tensorflow.org/images/GitHub-Mark-32px.png)](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L2049-L2078)[GitHub에서 소스 확인하기](https://www.github.com/wandb/client/tree/master/wandb/sdk/wandb_run.py#L2049-L2078)​​

지정된 제목 및 텍스트와 함께 알림을 시작합니다.

```text
alert(    title, text, level=None, wait_duration=None)
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>title (str): &#xC54C;&#xB9BC;&#xC758; &#xC774;&#xB984;&#xC73C;&#xB85C;,
          64&#xC790;&#xBCF4;&#xB2E4; &#xC9E7;&#xC544;&#xC57C; &#xD569;&#xB2C8;&#xB2E4;</p>
        <p>text (str): &#xC54C;&#xB9BC;&#xC758; &#xD14D;&#xC2A4;&#xD2B8; &#xBCF8;&#xBB38;&#xC785;&#xB2C8;&#xB2E4;</p>
        <p>level (str or wandb.AlertLevel, optional): &#xC0AC;&#xC6A9;&#xD560; &#xC54C;&#xB9BC;
          &#xC218;&#xC900;&#xC73C;&#xB85C; &#xB2E4;&#xC74C; &#xC911; &#xD558;&#xB098;&#xC785;&#xB2C8;&#xB2E4;:
          `INFO`(&#xC815;&#xBCF4;), `WARN`(&#xACBD;&#xBCF4;), or `ERROR`(&#xC624;&#xB958;)</p>
        <p>wait_duration (int, float, or timedelta, optional): &#xC774; &#xC81C;&#xBAA9;&#xC758;
          &#xB2E4;&#xB978; &#xC54C;&#xB9BC;&#xC744; &#xBCF4;&#xB0B4;&#xAE30; &#xC804;
          &#xB300;&#xAE30;&#xD574;&#xC57C; &#xD558;&#xB294; &#xC2DC;&#xAC04;(&#xCD08;)&#xC785;&#xB2C8;&#xB2E4;.</p>
      </td>
    </tr>
  </tbody>
</table>

  


