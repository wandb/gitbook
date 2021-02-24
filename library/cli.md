---
description: '명령 줄 인터페이스로 로그인 및 코드 상태 복원, 로컬 디렉토리를 서버에 동기화, 그리고 초매개변수 스윕을 실행하실 수 있습니다'
---

# Command Line Interface

`pip install wandb`를 실행하신 후, **wandb**라는 새 명령어를 사용하실 수 있어야 합니다.  


다음의 하위 명령어를 사용하실 수 있습니다:

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD558;&#xC704; &#xBA85;&#xB839;&#xC5B4;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">docs</td>
      <td style="text-align:left">&#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xC5D0;&#xC11C; &#xBB38;&#xC11C;&#xB97C;
        &#xC5FD;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">init</td>
      <td style="text-align:left">W&amp;B&#xB85C; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xB97C; &#xAD6C;&#xC131;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">login</td>
      <td style="text-align:left">W&amp;B&#xC5D0; &#xB85C;&#xADF8;&#xC778;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">offline</td>
      <td style="text-align:left">
        <p>&#xC2E4;&#xD589; &#xB370;&#xC774;&#xD130;&#xB9CC; &#xB85C;&#xCEEC; &#xC2A4;&#xD1A0;&#xB9AC;&#xC9C0;&#xC5D0;
          &#xC800;&#xC7A5;&#xD569;&#xB2C8;&#xB2E4;. &#xD074;&#xB77C;&#xC6B0;&#xB4DC;
          &#xB3D9;&#xAE30;&#xD654;&#xB294; &#xD558;&#xC9C0; &#xC54A;&#xC2B5;&#xB2C8;&#xB2E4;.
          (off</p>
        <p>&#xB294; &#xB354; &#xC774;&#xC0C1; &#xC0AC;&#xC6A9;&#xB418;&#xC9C0; &#xC54A;&#xC74C;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">online</td>
      <td style="text-align:left">
        <p>&#xC774; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xC5D0;&#xC11C; W&amp;B&#xAC00;
          &#xD65C;&#xC131;&#xD654;&#xB418;&#xC5B4; &#xC788;&#xB294;&#xC9C0; &#xD655;&#xC778;&#xD569;&#xB2C8;&#xB2E4;.
          (on</p>
        <p>&#xC740; &#xB354; &#xC774;&#xC0C1; &#xC0AC;&#xC6A9;&#xB418;&#xC9C0; &#xC54A;&#xC74C;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">disabled</td>
      <td style="text-align:left">&#xD14C;&#xC2A4;&#xD2B8;&#xC5D0; &#xC720;&#xC6A9;&#xD55C; &#xBAA8;&#xB4E0;
        API &#xD638;&#xCD9C; &#xC0AC;&#xC6A9;&#xC744; &#xB055;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">enabled</td>
      <td style="text-align:left"><code>online</code>&#xACFC; &#xB3D9;&#xC77C;&#xD558;&#xBA70;, &#xD14C;&#xC2A4;&#xD2B8;&#xB97C;
        &#xB9C8;&#xCE5C; &#xD6C4; &#xC77C;&#xBC18;&#xC801;&#xC778; W&amp;B &#xB85C;&#xAE45;&#xC744;
        &#xC7AC;&#xAC1C;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">docker</td>
      <td style="text-align:left">&#xB3C4;&#xCEE4; &#xC774;&#xBBF8;&#xC9C0;(docker image) &#xC2E4;&#xD589;,
        cwd &#xB9C8;&#xC6B4;&#xD2B8; &#xBC0F; wandb&#xAC00; &#xC124;&#xCE58;&#xB418;&#xC5C8;&#xB294;&#xC9C0;
        &#xD655;&#xC778;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">docker-run</td>
      <td style="text-align:left">W&amp;B &#xD658;&#xACBD; &#xBCC0;&#xC218;&#xB97C; &#xB3C4;&#xCEE4; &#xC2E4;&#xD589;
        &#xBA85;&#xB839;(docker run command)&#xC5D0; &#xCD94;&#xAC00;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">projects</td>
      <td style="text-align:left">&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758; &#xB9AC;&#xC2A4;&#xD2B8;&#xB97C;
        &#xC791;&#xC131;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">pull</td>
      <td style="text-align:left">W&amp;B&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xC744; &#xC704;&#xD55C; &#xD30C;&#xC77C;&#xC744;
        &#xAC00;&#xC838;&#xC635;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">restore</td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xC5D0; &#xB300;&#xD55C; &#xCF54;&#xB4DC; &#xBC0F; &#xAD6C;&#xC131;
        &#xC0C1;&#xD0DC;(config state)&#xB97C; &#xBCF5;&#xC6D0;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">run</td>
      <td style="text-align:left">python&#xD504;&#xB85C;&#xADF8;&#xB7A8;&#xC774; &#xC544;&#xB2CC; &#xB2E4;&#xB978;
        &#xD504;&#xB85C;&#xADF8;&#xB7A8;&#xC744; &#xC2E4;&#xD589;&#xD569;&#xB2C8;&#xB2E4;.
        &#xD30C;&#xC774;&#xC120;&#xC758; &#xACBD;&#xC6B0;, wandb.init()&#xC744;
        &#xC0AC;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">runs</td>
      <td style="text-align:left">&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758; &#xC2E4;&#xD589; &#xB9AC;&#xC2A4;&#xD2B8;&#xB97C;
        &#xC791;&#xC131;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">sync</td>
      <td style="text-align:left"><b>tfevents </b>&#xB610;&#xB294; &#xC774;&#xC804; &#xC2E4;&#xD589; &#xD30C;&#xC77C;&#xC744;
        &#xD3EC;&#xD568;&#xD55C; &#xB85C;&#xCEEC; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xB97C;
        &#xB3D9;&#xAE30;&#xD654;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">&#xD604;&#xC7AC; &#xB514;&#xB809;&#xD1A0;&#xB9AC; &#xC0C1;&#xD0DC;&#xC758;
        &#xB9AC;&#xC2A4;&#xD2B8;&#xB97C; &#xC791;&#xC131;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left">sweep</td>
      <td style="text-align:left"><b>YAML &#xC815;&#xC758;(definition)&#xAC00; &#xC9C0;&#xC815;&#xB41C; &#xC0C8;&#xB85C;&#xC6B4; &#xC2A4;&#xC715;(sweep)&#xC744; &#xC0DD;&#xC131;&#xD569;&#xB2C8;&#xB2E4;</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">agent</td>
      <td style="text-align:left">&#xC2A4;&#xC715;&#xC5D0;&#xC11C; &#xD504;&#xB85C;&#xADF8;&#xB7A8;&#xC744;
        &#xC2E4;&#xD589;&#xD558;&#xB3C4;&#xB85D; &#xC5D0;&#xC774;&#xC804;&#xD2B8;&#xB97C;
        &#xC2DC;&#xC791;&#xD569;&#xB2C8;&#xB2E4;</td>
    </tr>
  </tbody>
</table>

##  **코스 상태 복원하기**

 지정된 실행을 실행할 때, `restore`를 사용하셔서 코드 상태로 돌아갑니다.

###  **예시**

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

 **코드 상태를 어떻게 캡처하나요?**

  스크립트에서 `wandb.init`가 호출되면, 코드가 git repository에 있는 경우 링크가 마지막 git 커밋\(commit\)에 저장됩니다. 또한, 원격 상태에서 동기화되지 않은 변경 사항이나 커밋 되지 않은 변경사항이 있는 경우 diff 패치\(patch\) 또한 생성됩니다.

