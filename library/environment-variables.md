# Environment Variables

  
자동화된 환경에서 스크립트를 실행할 때, 스크립트 실행 전 또는 스크립트 내에서 환경 변수가 설정된 **wandb**를 제어하실 수 있습니다.

```bash
# This is secret and shouldn't be checked into version control
WANDB_API_KEY=$YOUR_API_KEY
# Name and notes optional
WANDB_NAME="My first run"
WANDB_NOTES="Smaller learning rate, more regularization."
```

```bash
# Only needed if you don't checkin the wandb/settings file
WANDB_ENTITY=$username
WANDB_PROJECT=$project
```

```python
# If you don't want your script to sync to the cloud
os.environ['WANDB_MODE'] = 'dryrun'
```

##  **선택적 환경 변수**

다음과 같은 선택적 환경 변수를 사용해서 원격 머신에 인증 설정과 같은 작업을 수행하실 수 있습니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variable name</th>
      <th style="text-align:left">Usage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>WANDB_API_KEY</b>
      </td>
      <td style="text-align:left">&#xACC4;&#xC815;&#xC5D0; &#xC5F0;&#xACB0;&#xB41C; &#xC778;&#xC99D; &#xD0A4;&#xB97C;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;. <a href="https://app.wandb.ai/settings">&#xC5EC;&#xB7EC;&#xBD84;&#xC758; &#xC124;&#xC815; &#xD398;&#xC774;&#xC9C0;(settings page)</a>&#xC5D0;&#xC11C;
        &#xD0A4;&#xB97C; &#xD655;&#xC778;&#xD558;&#xC2E4; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.<code> wandb login</code>&#xC774;
        &#xC6D0;&#xACA9;&#xBA38;&#xC2E0;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB418;&#xC9C0;
        &#xC54A;&#xB294; &#xACBD;&#xC6B0; &#xC774; &#xAC12;&#xC744; &#xC124;&#xC815;&#xD558;&#xC154;&#xC57C;
        &#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_BASE_URL</b>
      </td>
      <td style="text-align:left">&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xC2DC;&#xB294; &#xACBD;&#xC6B0;, &#xC774;
        &#xD658;&#xACBD; &#xBCC0;&#xC218;&#xB97C; <code>http://YOUR_IP:YOUR_PORT</code>&#xC5D0;
        &#xC124;&#xC815;&#xD558;&#xC154;&#xC57C; &#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NAME</b>
      </td>
      <td style="text-align:left">&#xC0AC;&#xB78C;&#xC774; &#xC77D;&#xC744; &#xC218; &#xC788;&#xB294; &#xD615;&#xC2DD;&#xC758;
        &#xC2E4;&#xD589; &#xC774;&#xB984;. &#xC124;&#xC815;&#xB418;&#xC9C0; &#xC54A;&#xC740;
        &#xACBD;&#xC6B0;, &#xC784;&#xC758;&#xB85C; &#xC0DD;&#xC131;&#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NOTES</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xC5D0; &#xB300;&#xD55C; &#xBA54;&#xBAA8;&#xAC00; &#xAE38;&#xC5B4;&#xC9D1;&#xB2C8;&#xB2E4;.
        &#xB9C8;&#xD06C;&#xB2E4;&#xC6B4;(Markdown)&#xC774; &#xD5C8;&#xC6A9;&#xB418;&#xBA70;,
        &#xCD94;&#xD6C4;&#xC5D0; UI&#xC5D0;&#xC11C; &#xD3B8;&#xC9D1;&#xD560; &#xC218;
        &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ENTITY</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xACFC; &#xC5F0;&#xACB0;&#xB41C; &#xAC1C;&#xCCB4;. <code>wandb init</code>&#xC744;
        &#xD6C8;&#xB828; &#xC2A4;&#xD06C;&#xB9BD;&#xD2B8; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xC5D0;&#xC11C;
        &#xC2E4;&#xD589;&#xD558;&#xACE0; &#xC788;&#xB294; &#xACBD;&#xC6B0;, <em>wandb&#xB77C;&#xB294; &#xC774;&#xB984;&#xC758; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xAC00; &#xC0DD;&#xC131;&#xB418;&#xACE0;, &#xC18C;&#xC2A4; &#xC81C;&#xC5B4;(source control)&#xB85C; &#xCCB4;&#xD06C;&#xC778; &#xD560; &#xC218; &#xC788;&#xB294; &#xAE30;&#xBCF8;&#xAC12; &#xAC1C;&#xCCB4;(default entity)&#xAC00; &#xC800;&#xC7A5;&#xB429;&#xB2C8;&#xB2E4;. &#xD574;&#xB2F9; &#xD30C;&#xC77C;&#xC744; &#xC0DD;&#xC131;&#xD558;&#xACE0; &#xC2F6;&#xC9C0; &#xC54A;&#xAC70;&#xB098;, &#xD30C;&#xC77C;&#xC744; &#xC624;&#xBC84;&#xB77C;&#xC774;&#xB4DC;(override)&#xD558;&#xACE0; &#xC2F6;&#xC73C;&#xC2DC;&#xB2E4;&#xBA74;, &#xC774; &#xD658;&#xACBD; &#xBCC0;&#xC218;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xC2E4; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_USERNAME</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xACFC; &#xC5F0;&#xACB0;&#xB41C; &#xD300; &#xAD6C;&#xC131;&#xC6D0;&#xC758;
        &#xC0AC;&#xC6A9;&#xC790; &#xC774;&#xB984;. &#xC774;&#xB97C; &#xD1B5;&#xD574;
        &#xC11C;&#xBE44;&#xC2A4; &#xACC4;&#xC815; API &#xD0A4;&#xC640; &#xD568;&#xAED8;
        &#xC0AC;&#xC6A9;&#xD574;&#xC11C; &#xD300; &#xAD6C;&#xC131;&#xC6D0;&#xC5D0;&#xAC8C;
        &#xC790;&#xB3D9;&#xD654; &#xC2E4;&#xD589;&#xC744; &#xD65C;&#xC131;&#xD654;
        &#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_PROJECT</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xACFC; &#xAD00;&#xB828;&#xB41C; &#xD504;&#xB85C;&#xC81D;&#xD2B8;. <code>wandb init</code>&#xB85C;
        &#xC124;&#xC815;&#xD558;&#xC2E4; &#xC218;&#xB3C4; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
        &#xD558;&#xC9C0;&#xB9CC;, &#xD574;&#xB2F9; &#xD658;&#xACBD; &#xBCC0;&#xC218;&#xB294;
        &#xAC12;(value)&#xB97C; &#xC624;&#xBC84;&#xB77C;&#xC774;&#xB4DC;(override)&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_MODE</b>
      </td>
      <td style="text-align:left">&#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C;, &#xC774; &#xAC12;&#xC740; &#xACB0;&#xACFC;&#xB97C;
        wandb&#xC5D0; &#xC800;&#xC7A5;&#xD558;&#xB294; run(&#xC2E4;&#xD589;)&#xC73C;&#xB85C;
        &#xC124;&#xC815;&#xB429;&#xB2C8;&#xB2E4;. &#xC2E4;&#xD589; &#xBA54;&#xD0C0;&#xB354;&#xC774;&#xD130;&#xB97C;
        &#xB85C;&#xCEEC;&#xC5D0;&#xB9CC; &#xC800;&#xC7A5;&#xD558;&#xACE0; &#xC2F6;&#xC73C;&#xC2DC;&#xB2E4;&#xBA74;,
        &#xC774; &#xBCC0;&#xC218;&#xB97C; dryrun(&#xB4DC;&#xB77C;&#xC774;&#xB7F0;)&#xC73C;&#xB85C;
        &#xC124;&#xC815;&#xD558;&#xC2E4; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_TAGS</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xC5D0; &#xC801;&#xC6A9; &#xB420; &#xD0DC;&#xADF8;&#xC758;
        &#xCF64;&#xB9C8;&#xB85C; &#xAD6C;&#xBD84;&#xB41C; &#xB9AC;&#xC2A4;&#xD2B8;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DIR</b>
      </td>
      <td style="text-align:left"><em> </em>&#xD6C8;&#xB828; &#xC2A4;&#xD06C;&#xB9BD;&#xD2B8;&#xC640; &#xAD00;&#xB828;&#xB41C;
        wandb &#xB514;&#xB809;&#xD1A0;&#xB9AC; &#xB300;&#xC2E0; &#xC0DD;&#xC131;&#xB41C;
        &#xBAA8;&#xB4E0; &#xD30C;&#xC77C;&#xC744; &#xC800;&#xC7A5;&#xD558;&#xC2DC;&#xB824;&#xBA74;
        &#xC774; &#xAC12;&#xC744; &#xC808;&#xB300; &#xACBD;&#xB85C;&#xC5D0; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.
        &#xD558;&#xC9C0;&#xB9CC;, &#xC774; &#xB514;&#xB809;&#xD1A0;&#xB9AC;&#xAC00;
        &#xC874;&#xC7AC;&#xD558;&#xACE0; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xAC00;
        &#xC2E4;&#xD589;&#xB418;&#xB294; &#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xC5EC;&#xAE30;&#xB85C;
        &#xC791;&#xC131;&#xD560; &#xC218; &#xC788;&#xB294;&#xC9C0; &#xD655;&#xC778;&#xD558;&#xC2DC;&#xAE30;
        &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RESUME</b>
      </td>
      <td style="text-align:left">&#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C; never(&#xC5C6;&#xC74C;)&#xB85C;
        &#xC124;&#xC815;&#xB429;&#xB2C8;&#xB2E4;. auto(&#xC790;&#xB3D9;)&#xB85C;
        &#xC124;&#xC815;&#xD558;&#xC2DC;&#xBA74;, wandb&#xB294; &#xC2E4;&#xD328;&#xD55C;
        &#xC2E4;&#xD589;&#xC744; &#xC790;&#xB3D9;&#xC73C;&#xB85C; &#xC7AC;&#xAC1C;&#xD569;&#xB2C8;&#xB2E4;.
        must(&#xD544;&#xC218;)&#xB85C; &#xC124;&#xC815;&#xD558;&#xC2DC;&#xBA74;
        &#xC2DC;&#xC791; &#xC2DC; &#xC2E4;&#xD589;&#xC774; &#xC874;&#xC7AC;&#xD558;&#xB3C4;&#xB85D;
        &#xAC15;&#xC81C;&#xD569;&#xB2C8;&#xB2E4;. &#xD56D;&#xC0C1; &#xACE0;&#xC720;&#xD55C;
        ID&#xB97C; &#xC0DD;&#xC131;&#xD558;&#xACE0; &#xC2F6;&#xC73C;&#xC2DC;&#xB2E4;&#xBA74;,
        allow(&#xD5C8;&#xC6A9;)&#xC73C;&#xB85C; &#xC124;&#xC815;&#xD558;&#xACE0;
        &#xD56D;&#xC0C1; <b>WANDB_RUN_ID</b>&#xB97C; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RUN_ID</b>
      </td>
      <td style="text-align:left">&#xC2A4;&#xD06C;&#xB9BD;&#xD2B8;&#xC758; &#xB2E8;&#xC77C; &#xC2E4;&#xD589;&#xC5D0;
        &#xC0C1;&#xC751;&#xD558;&#xB294; &#xC804;&#xCCB4;&#xC801;&#xC73C;&#xB85C;
        &#xACE0;&#xC720;&#xD55C; &#xC2A4;&#xD2B8;&#xB9C1;(globally unique string)
        (&#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xB2F9;)&#xC5D0; &#xC774; &#xBCC0;&#xC218;&#xB97C;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;. &#xBC18;&#xB4DC;&#xC2DC; 64&#xC790;
        &#xC774;&#xD558;&#xC5EC;&#xC57C; &#xD569;&#xB2C8;&#xB2E4;. &#xBAA8;&#xB4E0;
        &#xBE44;&#xB2E8;&#xC5B4; &#xBB38;&#xC790;&#xB294; &#xB300;&#xC2DC;(dash)&#xB85C;
        &#xBCC0;&#xD658;&#xB429;&#xB2C8;&#xB2E4;. &#xB610;&#xD55C; &#xC774; &#xC124;&#xC815;&#xC744;
        &#xD1B5;&#xD574;, &#xC2E4;&#xD328;(failure)&#xAC00; &#xBC1C;&#xC0DD;&#xD55C;
        &#xACBD;&#xC6B0; &#xAE30;&#xC874; &#xC2E4;&#xD589;&#xC744; &#xC7AC;&#xAC1C;&#xD558;&#xB294;
        &#xB370; &#xC0AC;&#xC6A9;&#xD558;&#xC2E4; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_IGNORE_GLOBS</b>
      </td>
      <td style="text-align:left">&#xBB34;&#xC2DC;&#xD560; &#xD30C;&#xC77C; &#xAE00;&#xB85C;&#xBE0C;(globs)&#xC758;
        &#xCF64;&#xB9C8;&#xB85C; &#xAD6C;&#xBD84;&#xB41C; &#xB9AC;&#xC2A4;&#xD2B8;&#xB85C;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;. &#xC774;&#xB7EC;&#xD55C; &#xD30C;&#xC77C;&#xC740;
        &#xD074;&#xB77C;&#xC6B0;&#xB4DC;&#xC5D0; &#xB3D9;&#xAE30;&#xD654;&#xB418;&#xC9C0;
        &#xC54A;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ERROR_REPORTING</b>
      </td>
      <td style="text-align:left">wandb&#xAC00; &#xC624;&#xB958; &#xCD94;&#xC801; &#xC2DC;&#xC2A4;&#xD15C;&#xC5D0;
        &#xCE58;&#xBA85;&#xC801; &#xC624;&#xB958;&#xB97C; &#xB85C;&#xAE45;&#xD558;&#xC9C0;
        &#xBABB;&#xD558;&#xAC8C; &#xD558;&#xB824;&#xBA74; &#xC774; &#xAC12;&#xC744;
        false&#xB85C; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_SHOW_RUN</b>
      </td>
      <td style="text-align:left">&#xC6B4;&#xC601;&#xCCB4;&#xC81C;&#xC5D0;&#xC11C; &#xC9C0;&#xC6D0;&#xD558;&#xB294;
        &#xACBD;&#xC6B0; &#xC2E4;&#xD589; url&#xB85C; &#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xB97C;
        &#xC790;&#xB3D9;&#xC73C;&#xB85C; &#xC5F4;&#xB824;&#xBA74; &#xC774; &#xAC12;&#xC744; <b>true</b>&#xB85C;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DOCKER</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589; &#xBCF5;&#xC6D0;&#xC744; &#xD65C;&#xC131;&#xD654; &#xD558;&#xC2DC;&#xB824;&#xBA74;
        &#xB3C4;&#xCEE4; &#xC774;&#xBBF8;&#xC9C0; &#xB2E4;&#xC774;&#xC81C;&#xC2A4;&#xD2B8;(docker
        image digest)&#xC5D0; &#xC774; &#xAC12;&#xC744; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.
        &#xC774;&#xB294; <code>wandb docker </code>&#xBA85;&#xB801;&#xACFC; &#xD568;&#xAED8;
        &#xC790;&#xB3D9;&#xC73C;&#xB85C; &#xC124;&#xC815;&#xB429;&#xB2C8;&#xB2E4;.
        &#xC774;&#xBBF8;&#xC9C0; &#xB2E4;&#xC774;&#xC81C;&#xC2A4;&#xD2B8;&#xB97C;
        &#xC5BB;&#xC73C;&#xC2DC;&#xB824;&#xBA74; &#xB2E4;&#xC74C;&#xC744; &#xC2E4;&#xD589;&#xD569;&#xB2C8;&#xB2E4;. <code>wandb docker my/image/name:tag --digest</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DISABLE_CODE</b>
      </td>
      <td style="text-align:left">wandb&#xAC00; &#xC18C;&#xC2A4; &#xCF54;&#xB4DC;&#xC5D0; &#xB300;&#xD55C;
        &#xCC38;&#xC870;&#xB97C; &#xC800;&#xC7A5;&#xD558;&#xC9C0; &#xBABB;&#xD558;&#xAC8C;
        &#xD558;&#xB824;&#xBA74; true&#xB85C; &#xC774; &#xAC12;&#xC744; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ANONYMOUS</b>
      </td>
      <td style="text-align:left">&#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xBE44;&#xBC00; url&#xB85C; &#xC775;&#xBA85;
        &#xC2E4;&#xD589;&#xC744; &#xC0DD;&#xC131;&#xD560; &#xC218; &#xC788;&#xB3C4;&#xB85D;
        &#xD5C8;&#xC6A9;&#xD558;&#xC2DC;&#xB824;&#xBA74; &#xC774; &#xAC12;&#xC744;
        &quot;allow(&#xD5C8;&#xC6A9;)&quot;, &quot;never(&#xC5C6;&#xC74C;)&quot;,
        &#xB610;&#xB294; &quot;must(&#xD544;&#xC218;)&quot;&#xB85C; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONSOLE</b>
      </td>
      <td style="text-align:left">stdout / stderr &#xB85C;&#xAE45;&#xC744; &#xBE44;&#xD65C;&#xC131;&#xD654;
        &#xD558;&#xB824;&#xBA74; &#xC774; &#xAC12;&#xC744; &#x201C;off(&#xB054;)&#x201D;&#xC73C;&#xB85C;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;. &#xC774; &#xAE30;&#xB2A5;&#xC740;
        &#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C; &#xC774; &#xAE30;&#xB2A5;&#xC744;
        &#xC9C0;&#xC6D0;&#xD558;&#xB294; &#xD658;&#xACBD;&#xC5D0;&#xC11C;&#xB294;
        &#x201C;on(&#xCF2C;)&#x201D;&#xC73C;&#xB85C; &#xC124;&#xC815;&#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONFIG_PATHS</b>
      </td>
      <td style="text-align:left">
        <p>wandb.config&#xB85C; &#xB85C;&#xB529;&#xD560; yaml &#xD30C;&#xC77C;&#xC758;
          &#xCF64;&#xB9C8;&#xB85C; &#xAD6C;&#xBD84;&#xB41C; &#xBAA9;&#xB85D;&#xC785;&#xB2C8;&#xB2E4;.</p>
        <p><a href="https://docs.wandb.com/library/config#file-based-configs">config(&#xAD6C;&#xC131;)</a>&#xB97C;
          &#xCC38;&#xC870;&#xD558;&#xC138;&#xC694;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONFIG_DIR</b>
      </td>
      <td style="text-align:left">&#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C; ~/.config/wandb &#xC124;&#xC815;&#xB418;&#xC5B4;
        &#xC788;&#xC73C;&#xBA70;, &#xC774; &#xD658;&#xACBD; &#xBCC0;&#xC218;&#xB85C;
        &#xC774; &#xC704;&#xCE58;&#xB97C; &#xC624;&#xBC84;&#xB77C;&#xC774;&#xB4DC;(override)&#xD560;
        &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NOTEBOOK_NAME</b>
      </td>
      <td style="text-align:left">jupyter&#xC5D0;&#xC11C; &#xC2E4;&#xD589; &#xC911;&#xC778; &#xACBD;&#xC6B0;,
        notebook&#xC758; &#xC774;&#xB984;&#xC744; &#xC774; &#xBCC0;&#xC218;&#xC640;
        &#xD568;&#xAED8; &#xC124;&#xC815;&#xD558;&#xC2E4; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
        &#xC800;&#xD76C;&#xB294; &#xC790;&#xB3D9;&#xC73C;&#xB85C; &#xC774;&#xB97C;
        &#xAC10;&#xC9C0;&#xD558;&#xAE30; &#xC704;&#xD574; &#xC2DC;&#xB3C4;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_HOST</b>
      </td>
      <td style="text-align:left">&#xC2DC;&#xC2A4;&#xD15C;&#xC774; &#xC81C;&#xACF5;&#xD55C; &#xD638;&#xC2A4;&#xD2B8;&#xBA85;(hostname)&#xC744;
        &#xC0AC;&#xC6A9;&#xD558;&#xACE0; &#xC2F6;&#xC9C0; &#xC54A;&#xC740; &#xACBD;&#xC6B0;,
        wandb &#xC778;&#xD130;&#xD398;&#xC774;&#xC2A4;&#xC5D0; &#xD45C;&#xC2DC;&#xD560;
        &#xD638;&#xC2A4;&#xD2B8;&#xBA85;&#xC73C;&#xB85C; &#xC774; &#xAC12;&#xC744;
        &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_SILENT</b>
      </td>
      <td style="text-align:left">wandb log statement&#xB97C; &#xC74C;&#xC18C;&#xAC70; &#xD558;&#xB824;&#xBA74;
        &#xC774; &#xAC12;&#xC744; <b>true</b>&#xB85C; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.
        &#xC774;&#xB807;&#xAC8C; &#xC124;&#xC815; &#xB418;&#xBA74;, &#xBAA8;&#xB4E0;
        &#xB85C;&#xADF8;&#xAC00; <b>WANDB_DIR</b>/debug.log&#xB85C; &#xAE30;&#xB85D;&#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RUN_GROUP</b>
      </td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xC744; &#xC790;&#xB3D9;&#xC73C;&#xB85C; &#xADF8;&#xB8F9;&#xD654;
        &#xD560; &#xC2E4;&#xD5D8; &#xC774;&#xB984;&#xC744; &#xC9C0;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.
        &#xB354; &#xC790;&#xC138;&#xD55C; &#xC815;&#xBCF4;&#xB294; <a href="https://docs.wandb.com/library/advanced/grouping">&#xADF8;&#xB8E8;&#xD551;(grouping)</a>&#xC744;
        &#xCC38;&#xC870;&#xD558;&#xC2ED;&#xC2DC;&#xC624;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_JOB_TYPE</b>
      </td>
      <td style="text-align:left">&#xB2E4;&#xB978; &#xC720;&#xD615;&#xC758; &#xC2E4;&#xD589;&#xC744; &#xB098;&#xD0C0;&#xB0B4;&#xAE30;
        &#xC704;&#xD574; &#xC791;&#xC5C5; &#xC720;&#xD615;(&#xC608;: &quot;training(&#xD6C8;&#xB828;)&quot;
        or &quot;evaluation(&#xD3C9;&#xAC00;)&quot;)&#xC744; &#xC9C0;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.
        &#xB354; &#xC790;&#xC138;&#xD55C; &#xC815;&#xBCF4;&#xB294; <a href="https://docs.wandb.com/library/advanced/grouping">&#xADF8;&#xB8E8;&#xD551;(grouping)</a>&#xC744;
        &#xCC38;&#xC870;&#xD558;&#xC2ED;&#xC2DC;&#xC624;.</td>
    </tr>
  </tbody>
</table>

## **Singularity Environments\(특이성 환경\)** 

[특이성\(Singularity\)](https://singularity.lbl.gov/index.html)에서 컨테이너\(container\)를 실행하는 경우, 위의 변수 앞에 **SINGULARITYENV\_**를 추가하여 환경변수를 전달할 수 있습니다. 특이성 환경 변수에 관한 자세한 내용은 [여기](https://singularity.lbl.gov/docs-environment-metadata#environment)에서 확인하실 수 있습니다.

##  **AWS에서 실행하기**

배치\(batch\) 작업을 AWS에서 실행하는 경우, W&B 인증\(credentials\)로 간편하게 여러분의 머신을 인증하실 수 있습니다. [설정 페이지\(settings page\)](https://app.wandb.ai/settings)에서 API키를 가져와서 [AWS batch job spec](https://docs.aws.amazon.com/batch/latest/userguide/job_definition_parameters.html#parameters)에서 WANDB\_API\_KEY 환경 변수를 설정합니다.

## **공통 질문**

###  **자동 실행 및 서비스 계정**

 W&B로 로깅하는 실행을 시작하는 자동화된 테스트 또는 내부 툴이 있는 경우, 여러분의 팀 설정 페이지에서 **서비스 계정\(Service Account\)**을 생성하십시오. 이를 통해서 자동화된 작업에 대한 서비스 API 키를 사용하실 수 있습니다. 서비스 계정 잡업을 특정 사용자에게 지정하고 싶으시다면, WANDB\_USER\_NAME 또는 WANDB\_USER\_EMAIL 환경 변수를 사용하실 수 있습니다.

![&#xD300; &#xC124;&#xC815; &#xD398;&#xC774;&#xC9C0;&#xC5D0;&#xC11C; &#xC790;&#xB3D9;&#xD654;&#xB41C; &#xC791;&#xC5C5;&#xC5D0; &#xB300;&#xD55C; &#xC11C;&#xBE44;&#xC2A4; &#xACC4;&#xC815;&#xC744; &#xC0DD;&#xC131;&#xD558;&#xC2ED;&#xC2DC;&#xC624;](../.gitbook/assets/image%20%2892%29.png)

자동화된 유닛 테스트\(unit test\)를 설정하는 경우, 이 기능은 계속적인 통합 및 TravisCI 또는 CircleCI과 같은 툴에 유용합니다.

### **환경 변수는 wandb.init\(\)로 전달되는 매개변수를 오버라이드\(override\)하나요?**

에 전달되는 전달인자는 환경변수보다 우선합니다. 환경 변수가 설정되지 않은 경우 시스템 기본값\(system default\)와 다른 기본값을 사용하고 싶으시다면 `wandb.init(dir=os.getenv("WANDB_DIR", my_default_override))`을 요청하실 수 있습니다.

### **로깅 끄기**

 `wandb off` 명령은 환경 변수, `WANDB_MODE=dryrun`을 설정합니다. 이를 통해서 여러분의 머신에서 원격 wandb 서버로 데이터가 동기화 되는 것을 중단할 수 있습니다. 여러 프로젝트가 있는 경우, 모든 프로젝트는 로깅된 데이터가 W&B 서버로 동기화되는 것을 중단합니다.

 경고 메시지를 끄려면 다음을 수행합니다:

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.WARNING)
```

### Multiple wandb users on shared machines

If you're using a shared machine and another person is a wandb user, it's easy to make sure your runs are always logged to the proper account. Set the [WANDB\_API\_KEY environment variable](environment-variables.md) to authenticate. If you source it in your env, when you log in you'll have the right credentials, or you can set the environment variable from your script.

Run this command `export WANDB_API_KEY=X` where X is your API key. When you're logged in, you can find your API key at [wandb.ai/authorize](https://app.wandb.ai/authorize).

