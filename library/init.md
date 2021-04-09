---
description: '모델 메트릭을 로그하는 훈련 루프 이전에, 새로운 실행을 시작할 때마다 wandb.init()을 호출합니다.'
---

# wandb.init\(\)

모델 메트릭을 로그하는 훈련 루프 이전에, 새로운 실행을 시작할 때마다 `wandb.init()`을 호출합니다. 스크립트 시작 시에 `wandb.init()`를 한 번 호출하여 새 작업을 초기화합니다. 이를 통해 W&B에 새로운 실행을 생성하고, 데이터를 동기화하는 백그라운드 프로세스를 시작합니다. 프라이빗 클라우드 또는 W&B 로컬 설치가 필요한 경우, 저희는 [자체 호스팅](https://docs.wandb.ai/v/ko/self-hosted) 서비스에서 이를 제공합니다. `wandb.init()`은 [실행](https://docs.wandb.com/ref/export-api/api#run) 객체를 반환하고 `wandb.run`와 함께 실행 객체에 액세스할 수도 있습니다.

###  생략 가능한 전달인자

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC804;&#xB2EC;&#xC778;&#xC790;</th>
      <th style="text-align:left">&#xC720;&#xD615;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">project</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#xC0C8;&#xB85C;&#xC6B4; &#xC2E4;&#xD589;&#xC744; &#xC804;&#xC1A1;&#xD558;&#xB294;
        &#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC758; &#xC774;&#xB984;. &#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xAC00;
        &#xC9C0;&#xC815;&#xB418;&#xC9C0; &#xC54A;&#xC740; &#xACBD;&#xC6B0;, &#xC2E4;&#xD589;&#xC740;
        &#xAE30;&#xBCF8;&#xAC12; &#xAC1C;&#xCCB4;&#xC758; &#x201C;uncategorized&#x201D;(&#xBC94;&#xC8FC;&#xD654;&#xB418;&#xC9C0;
        &#xC54A;&#xC740;) &#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xD569;&#xB2C8;&#xB2E4;.
        &#x201C;default location to create new projects&#x201D;(&#xC0C8; &#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xB97C;
        &#xC0DD;&#xC131;&#xD560; &#xAE30;&#xBCF8; &#xC704;&#xCE58;) &#xC758;
        <a
        href="https://wandb.ai/settings">&#xC124;&#xC815; &#xD398;&#xC774;&#xC9C0;</a>&#xC5D0;&#xC11C; &#xAE30;&#xBCF8;&#xAC12;
          &#xAC1C;&#xCCB4;&#xB97C; &#xBCC0;&#xACBD;&#xD558;&#xAC70;&#xB098; <code>entity </code>&#xC804;&#xB2EC;
          &#xC778;&#xC790;&#xB97C; &#xC124;&#xC815;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">entity</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#xAC1C;&#xCCB4;&#xB294; &#xC2E4;&#xD589;&#xC744; &#xC804;&#xC1A1;&#xD558;&#xB294;
        &#xC0AC;&#xC6A9;&#xC790; &#xC774;&#xB984; &#xB610;&#xB294; &#xD300; &#xC774;&#xB984;&#xC785;&#xB2C8;&#xB2E4;.
        &#xC774; &#xAC1C;&#xCCB4;&#xB294; &#xC2E4;&#xD589;&#xC744; &#xC804;&#xC1A1;&#xD558;&#xAE30;
        &#xC804;&#xC5D0; &#xBC18;&#xB4DC;&#xC2DC; &#xC874;&#xC7AC;&#xD574;&#xC57C;
        &#xD558;&#xBA70;,<b> </b>&#xB530;&#xB77C;&#xC11C; &#xC2E4;&#xD589;&#xC744;
        &#xB85C;&#xADF8; &#xD558;&#xAE30; &#xC804;&#xC5D0; UI&#xC5D0;&#xC11C; &#xACC4;&#xC815;
        &#xB610;&#xB294; &#xD300;&#xC744; &#xC0DD;&#xC131;&#xD558;&#xC154;&#xC57C;
        &#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">save_code</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left"><b> </b>&#xBA54;&#xC778; &#xC2A4;&#xD06C;&#xB9BD;&#xD2B8; &#xB610;&#xB294;
        &#xB178;&#xD2B8;&#xBD81;&#xC744; W&amp;B&#xC5D0; &#xC800;&#xC7A5;&#xD558;&#xB824;&#xBA74;
        &#xC774;&#xAC83;&#xC744; &#xCF2D;&#xB2C8;&#xB2E4;. &#xC2E4;&#xD5D8; &#xC7AC;&#xD604;&#xC131;&#xC744;
        &#xD5A5;&#xC0C1;&#xD558;&#xACE0; UI&#xC5D0;&#xC11C; &#xC2E4;&#xD5D8; &#xAC04;
        &#xCF54;&#xB4DC; &#xB514;&#xD551;&#xC5D0; &#xC720;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;.
        &#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C; &#xAEBC;&#xC838;&#xC788;&#xC73C;&#xB098;,
        <a
        href="https://wandb.ai/settings">&#xC124;&#xC815;</a>&#xC5D0;&#xC11C; &#xC774;&#xB97C; &#xCF2C;&#xC73C;&#xB85C;
          &#xBCC0;&#xACBD;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">group</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#xADF8;&#xB8F9;&#xC744; &#xC9C0;&#xC815;&#xD558;&#xC5EC; &#xAC1C;&#xBCC4;
        &#xC2E4;&#xD589;&#xC744; &#xB354; &#xD070; &#xC2E4;&#xD5D8;&#xC73C;&#xB85C;
        &#xAD6C;&#xC131;&#xD569;&#xB2C8;&#xB2E4;. &#xC608;&#xB97C; &#xB4E4;&#xC5B4;,
        k-fold &#xAD50;&#xCC28; &#xAC80;&#xC99D;&#xC744; &#xC218;&#xD589;&#xD558;&#xAC70;&#xB098;,
        &#xB2E4;&#xB978; &#xC5EC;&#xB7EC; &#xD14C;&#xC2A4;&#xD2B8; &#xC138;&#xD2B8;&#xC5D0;
        &#xB300;&#xD55C; &#xBAA8;&#xB378;&#xC744; &#xD6C8;&#xB828; &#xBC0F; &#xD3C9;&#xAC00;&#xD558;&#xB294;
        &#xB2E4;&#xC591;&#xD55C; &#xC791;&#xC5C5;&#xC774; &#xC788;&#xC744; &#xC218;
        &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;. &#xADF8;&#xB8F9;&#xC740; &#xC2E4;&#xD589;&#xC744;
        &#xD568;&#xAED8; &#xD558;&#xB098;&#xC758; &#xD070; &#xC804;&#xCCB4;&#xB85C;
        &#xAD6C;&#xC131;&#xD560; &#xC218; &#xC788;&#xC73C;&#xBA70;, UI&#xC5D0;&#xC11C;
        &#xC774;&#xB97C; &#xD1A0;&#xAE00;&#xD558;&#xC5EC; &#xCF1C;&#xAC70;&#xB098;
        &#xB04C; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;. &#xC790;&#xC138;&#xD55C;
        &#xB0B4;&#xC6A9;&#xC740; <a href="https://docs.wandb.ai/v/ko/library/grouping">&#xADF8;&#xB8F9;&#xD654;</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xC2DC;&#xAE30; &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">job_type</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#xC2E4;&#xD589; &#xC720;&#xD615;&#xC744; &#xC9C0;&#xC815;&#xD558;&#xBA70;,
        group&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xC5EC; &#xC2E4;&#xD589;&#xC744;
        &#xB354; &#xD070; &#xC2E4;&#xD5D8;&#xC73C;&#xB85C; &#xADF8;&#xB8F9;&#xD654;&#xD560;
        &#xB54C; &#xC720;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;. &#xC608;&#xB97C; &#xB4E4;&#xC5B4;,
        train &#xBC0F; eval&#xACFC; &#xAC19;&#xC740; &#xC791;&#xC5C5; &#xC720;&#xD615;&#xACFC;
        &#xD568;&#xAED8;, &#xADF8;&#xB8F9;&#xC5D0; &#xB2E4;&#xC591;&#xD55C; &#xC791;&#xC5C5;&#xC774;
        &#xC788;&#xC744; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;. &#xC774;&#xB97C;
        &#xC124;&#xC815;&#xD558;&#xBA74; UI&#xC5D0; &#xC720;&#xC0AC;&#xD55C; &#xC2E4;&#xD589;&#xC744;
        &#xC27D;&#xACE0; &#xAC04;&#xD3B8;&#xD558;&#xAC8C; &#xD544;&#xD130;&#xB9C1;
        &#xBC0F; &#xADF8;&#xB8F9;&#xD654;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
        &#xB530;&#xB77C;&#xC11C; &#xAC19;&#xC740; &#xC870;&#xAC74;&#xC5D0;&#xC11C;
        &#xBE44;&#xAD50;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">&#xC774; &#xC2E4;&#xD589;&#xC5D0; &#xB300;&#xD55C; &#xC9E7;&#xC740; &#xD45C;&#xC2DC;
        &#xC774;&#xB984;&#xC73C;&#xB85C;, &#xC774;&#xB97C; &#xD1B5;&#xD574; UI&#xC5D0;&#xC11C;
        &#xC2E4;&#xD589;&#xC744; &#xC2DD;&#xBCC4;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
        &#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C;, &#xD14C;&#xC774;&#xBE14;&#xC5D0;&#xC11C;&#xBD80;&#xD130;
        &#xCC28;&#xD2B8;&#xAE4C;&#xC9C0; &#xC2E4;&#xD589;&#xC744; &#xC27D;&#xAC8C;
        &#xC0C1;&#xD638; &#xCC38;&#xC870;&#xD560; &#xC218; &#xC788;&#xB3C4;&#xB85D;
        &#xC784;&#xC758;&#xC758; 2&#xB2E8;&#xC5B4; &#xC774;&#xB984;&#xC744; &#xC0DD;&#xC131;&#xD569;&#xB2C8;&#xB2E4;.
        &#xC774;&#xB7EC;&#xD55C; &#xC2E4;&#xD589; &#xC774;&#xB984;&#xC744; &#xC9E7;&#xAC8C;
        &#xC720;&#xC9C0;&#xD558;&#xBA74; &#xCC28;&#xD2B8; &#xBC94;&#xB840; &#xBC0F;
        &#xD14C;&#xC774;&#xBE14;&#xC744; &#xBCF4;&#xB2E4; &#xC27D;&#xAC8C; &#xD310;&#xB3C5;&#xD560;
        &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;. &#xCD08;&#xB9E4;&#xAC1C;&#xBCC0;&#xC218;&#xB97C;
        &#xC800;&#xC7A5;&#xD560; &#xC704;&#xCE58;&#xB97C; &#xCC3E;&#xB294; &#xACBD;&#xC6B0;,
        &#xC800;&#xD76C;&#xB294; <code>config </code>(&#xC544;&#xB798;)&#xC5D0;
        &#xC800;&#xC7A5;&#xD558;&#xC2E4; &#xAC83;&#xC744; &#xCD94;&#xCC9C;&#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">notes</td>
      <td style="text-align:left">str</td>
      <td style="text-align:left">git&#xC758; a &#x2013;m &#xCEE4;&#xBC0B; &#xBA54;&#xC2DC;&#xC9C0;&#xC640;
        &#xAC19;&#xC740; &#xC2E4;&#xD589;&#xC5D0; &#xAD00;&#xD55C; &#xC880; &#xB354;
        &#xC0C1;&#xC138;&#xD55C; &#xC124;&#xBA85;. &#xC774;&#xB97C; &#xD1B5;&#xD574;&#xC11C;
        &#xC774; &#xC2E4;&#xD589;&#xC744; &#xC2E4;&#xD589;&#xD560; &#xB54C; &#xC5B4;&#xB5A4;
        &#xC791;&#xC5C5;&#xC744; &#xC218;&#xD589;&#xD558;&#xACE0; &#xC788;&#xC5C8;&#xB294;&#xC9C0;
        &#xB85C;&#xADF8;&#xD558;&#xB294; &#xB370; &#xB3C4;&#xC6C0;&#xC774; &#xB429;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">config</td>
      <td style="text-align:left">dict</td>
      <td style="text-align:left">&#xBAA8;&#xB378;&#xC5D0; &#xB300;&#xD55C; &#xCD08;&#xB9E4;&#xAC1C;&#xBCC0;&#xC218;
        &#xB610;&#xB294; &#xB370;&#xC774;&#xD130; &#xD504;&#xB85C;&#xC138;&#xC2F1;&#xC5D0;
        &#xB300;&#xD55C; &#xC124;&#xC815;&#xACFC; &#xAC19;&#xC774; &#xC791;&#xC5C5;&#xC5D0;
        &#xC785;&#xB825;&#xC744; &#xC800;&#xC7A5;&#xD558;&#xB294; &#xC0AC;&#xC804;
        &#xAC19;&#xC740; &#xAC1D;&#xCCB4;. &#xAD6C;&#xC131;(config)&#xC740; &#xC2E4;&#xD589;&#xC744;
        &#xADF8;&#xB8F9;&#xD654;, &#xD544;&#xD130;&#xB9C1;, &#xC815;&#xB82C;&#xD558;&#xB294;&#xB370;
        &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB294; UI&#xC758; &#xD14C;&#xC774;&#xBE14;&#xC5D0;
        &#xD45C;&#xC2DC;&#xB429;&#xB2C8;&#xB2E4;. &#xD0A4; &#xC774;&#xB984;&#xC5D0;&#xB294;<code> . </code>&#xC744;
        &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC5C6;&#xC73C;&#xBA70;, &#xAC12;&#xC740;
        10MB &#xBBF8;&#xB9CC;&#xC774;&#xC5B4;&#xC57C; &#xD569;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">tags</td>
      <td style="text-align:left">str[]</td>
      <td style="text-align:left">&#xC2A4;&#xD2B8;&#xB9C1;&#xC758; &#xB9AC;&#xC2A4;&#xD2B8;&#xB85C;, UI&#xC5D0;&#xC11C;
        &#xC774; &#xC2E4;&#xD589;&#xC758; &#xD0DC;&#xADF8; &#xB9AC;&#xC2A4;&#xD2B8;&#xB97C;
        &#xB367;&#xBD99;&#xC785;&#xB2C8;&#xB2E4;. &#xD0DC;&#xADF8;&#xB294; &#xC2E4;&#xD589;&#xC744;
        &#xD568;&#xAED8; &#xAD6C;&#xC131;&#xD558;&#xAC70;&#xB098; &#x201C;baseline&#x201D;
        &#xB610;&#xB294; &#x201C;production&#x201D;&#xACFC; &#xAC19;&#xC740; &#xC784;&#xC2DC;
        &#xB77C;&#xBCA8;&#xC744; &#xC801;&#xC6A9;&#xD560; &#xB54C; &#xC720;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;.
        UI&#xC5D0;&#xC11C; &#xC27D;&#xAC8C; &#xD0DC;&#xADF8;&#xB97C; &#xCD94;&#xAC00;
        &#xBC0F; &#xC81C;&#xAC70;&#xD560; &#xC218; &#xC788;&#xC73C;&#xBA70;, &#xD2B9;&#xC815;
        &#xD0DC;&#xADF8;&#xC640; &#xD568;&#xAED8; &#xC2E4;&#xD589;&#xB9CC; &#xD544;&#xD130;&#xB97C;
        &#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">dir</td>
      <td style="text-align:left">path</td>
      <td style="text-align:left">&#xC544;&#xD2F0;&#xD329;&#xD2B8;&#xC5D0;&#xC11C; <code>download()</code>&#xB97C;
        &#xD638;&#xCD9C;&#xD560; &#xB54C;, &#xB2E4;&#xC6B4;&#xB85C;&#xB4DC;&#xD55C;
        &#xD30C;&#xC77C;&#xC774; &#xC800;&#xC7A5;&#xB418;&#xB294; &#xB514;&#xB809;&#xD130;&#xB9AC;&#xC785;&#xB2C8;&#xB2E4;.
        &#xAE30;&#xBCF8;&#xAC12;&#xC740;<code> ./wandb directory</code>&#xC785;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">sync_tensorboard</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#xBAA8;&#xB4E0; TensorBoard &#xB85C;&#xADF8;&#xB97C; &#xAE30;&#xB85D;&#xC744;
        W&amp;B&#xC5D0; &#xBCF5;&#xC0AC;&#xD560;&#xC9C0; &#xC5EC;&#xBD80;&#xB97C;
        &#xB098;&#xD0C0;&#xB0C5;&#xB2C8;&#xB2E4;. &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
        <a
        href="https://docs.wandb.ai/v/ko/integrations/tensorboard">Tensorboard</a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xC2DC;&#xAE30; &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;
          (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">resume</td>
      <td style="text-align:left">bool or str</td>
      <td style="text-align:left">True&#xC778; &#xACBD;&#xC6B0;, &#xC2E4;&#xD589;&#xC774; &#xC790;&#xB3D9;&#xC73C;&#xB85C;
        &#xC7AC;&#xAC1C;&#xB429;&#xB2C8;&#xB2E4;. &#xC2E4;&#xD589;&#xC744; &#xC218;&#xB3D9;&#xC73C;&#xB85C;
        &#xC7AC;&#xAC1C;&#xD558;&#xB824;&#xBA74; &#xACE0;&#xC720;&#xD55C; &#xC2E4;&#xD589;
        ID&#xB85C; &#xC124;&#xC815;&#xD558;&#xC2DC;&#xAE30; &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;.
        &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740; <a href="https://docs.wandb.ai/v/ko/library/resuming">&#xC7AC;&#xAC1C;</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xC2DC;&#xAE30; &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;.
        (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">reinit</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#xB3D9;&#xC77C; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xC5D0;&#xC11C; &#xC5EC;&#xB7EC; <code>wandb.init() </code>&#xD638;&#xCD9C;&#xC744;
        &#xD5C8;&#xC6A9;&#xD560;&#xC9C0; &#xC5EC;&#xBD80;&#xB97C; &#xB098;&#xD0C0;&#xB0C5;&#xB2C8;&#xB2E4;.
        (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">anonymous</td>
      <td style="text-align:left">&quot;allow&quot; &quot;never&quot; &quot;must&quot;</td>
      <td style="text-align:left">
        <p></p>
        <p>&#xC775;&#xBA85; &#xB85C;&#xADF8;&#xB97C; &#xAE30;&#xB85D; &#xD5C8;&#xC6A9;
          &#xB610;&#xB294; &#xBE44;&#xD5C8;&#xC6A9; &#x2014; &#xACC4;&#xC815;&#xC744;
          &#xC0DD;&#xC131;&#xD558;&#xC9C0; &#xC54A;&#xACE0; W&amp;B &#xD074;&#xB77C;&#xC6B0;&#xB4DC;&#xC5D0;&#xC11C;
          &#xC2E4;&#xD589; &#xCD94;&#xC801;&#xD560; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.
          <br
          />
        </p>
        <ul>
          <li><b>&quot;allow&quot;(&#xD5C8;&#xC6A9;)</b>&#xC744; &#xC120;&#xD0DD;&#xD558;&#xC2DC;&#xBA74;
            &#xB85C;&#xADF8;&#xC778;&#xD55C; &#xC0AC;&#xC6A9;&#xC790;&#xB4E4;&#xC774;
            &#xC790;&#xC2E0;&#xC758; &#xACC4;&#xC815;&#xC73C;&#xB85C; &#xC2E4;&#xD589;&#xC744;
            &#xCD94;&#xC801;&#xD558;&#xC9C0;&#xB9CC;, W&amp;B &#xACC4;&#xC815; &#xC5C6;&#xC774;
            &#xC2A4;&#xD06C;&#xB9BD;&#xD2B8;&#xB97C; &#xC2E4;&#xD589;&#xD558;&#xB294;
            &#xB2E4;&#xB978; &#xC0AC;&#xB78C;&#xB4E4;&#xC774; UI&#xC758; &#xCC28;&#xD2B8;&#xB97C;
            &#xBCFC; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</li>
          <li><b>&quot;never&quot;(&#xD5C8;&#xC6A9; &#xC548; &#xD568;)</b>&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xB824;&#xBA74;
            &#xC2E4;&#xC218;&#xB85C; &#xC775;&#xBA85; &#xC2E4;&#xD589;&#xC744; &#xC0DD;&#xC131;&#xD558;&#xC9C0;
            &#xC54A;&#xB3C4;&#xB85D; &#xC2E4;&#xD589;&#xC744; &#xCD94;&#xC801;&#xD558;&#xAE30;
            &#xC804;&#xC5D0; W&amp;B &#xACC4;&#xC815;&#xC744; &#xC5F0;&#xB3D9;&#xD558;&#xC154;&#xC57C;
            &#xD569;&#xB2C8;&#xB2E4;.</li>
          <li><b>&quot;must&quot;(&#xBC18;&#xB4DC;&#xC2DC; &#xD5C8;&#xC6A9;)</b>&#xC744;
            &#xC120;&#xD0DD;&#xD558;&#xC2DC;&#xBA74; &#xC0AC;&#xC6A9;&#xC790; &#xACC4;&#xC815;
            &#xB300;&#xC2E0; &#xC775;&#xBA85; &#xACC4;&#xC815;&#xC73C;&#xB85C; &#xC2E4;&#xD589;&#xC744;
            &#xC804;&#xC1A1;&#xD569;&#xB2C8;&#xB2E4;.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">force</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#xC2A4;&#xD06C;&#xB9BD;&#xD2B8;&#xB97C; &#xC2E4;&#xD589;&#xD560; &#xB54C;
        &#xC0AC;&#xC6A9;&#xC790;&#xB97C; wandb&#xC5D0; &#xAC15;&#xC81C; &#xB85C;&#xADF8;&#xC778;
        &#xC2DC;&#xD0AC;&#xC9C0;&#xC5D0; &#xB300;&#xD55C; &#xC5EC;&#xBD80; (&#xAE30;&#xBCF8;&#xAC12;:
        Fasle)</td>
    </tr>
    <tr>
      <td style="text-align:left">magic</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#xC2A4;&#xD06C;&#xB9BD;&#xD2B8;&#xB97C; &#xC790;&#xB3D9; &#xC124;&#xC815;&#xD560;&#xC9C0;
        &#xC5EC;&#xBD80;&#xC774;&#xBA70;, wandb &#xCF54;&#xB4DC; &#xCD94;&#xAC00;
        &#xC5C6;&#xC774; &#xC2E4;&#xD589;&#xC758; &#xC138;&#xBD80; &#xC0AC;&#xD56D;&#xC744;
        &#xCEA1;&#xCC98;&#xD560;&#xC9C0;&#xC5D0; &#xB300;&#xD55C; &#xC5EC;&#xBD80;.
        (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">unique str</td>
      <td style="text-align:left">&#xC2E4;&#xD589;&#xC5D0; &#xB300;&#xD55C; &#xACE0;&#xC720;&#xD55C; ID&#xB85C;
        <a
        href="https://docs.wandb.ai/v/ko/library/resuming">&#xC7AC;&#xAC1C;</a>&#xC5D0; &#xC0AC;&#xC6A9;&#xB429;&#xB2C8;&#xB2E4;.
          &#xD504;&#xB85C;&#xC81D;&#xD2B8;&#xC5D0;&#xC11C; &#xBC18;&#xB4DC;&#xC2DC;
          &#xACE0;&#xC720;&#xD55C; &#xAC83;&#xC774;&#xC5B4;&#xC57C; &#xD558;&#xBA70;,
          &#xC2E4;&#xD589;&#xC744; &#xC0AD;&#xC81C;&#xD558;&#xBA74; &#xD574;&#xB2F9;
          ID&#xB97C; &#xB2E4;&#xC2DC; &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC5C6;&#xC2B5;&#xB2C8;&#xB2E4;.
          &#xC9E7;&#xC740; &#xC124;&#xBA85; &#xC774;&#xB984;&#xC758; &#xACBD;&#xC6B0;
          &#xC774;&#xB984; &#xC601;&#xC5ED;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xAC70;&#xB098;
          &#xC5EC;&#xB7EC; &#xC2E4;&#xD589;&#xC744; &#xBE44;&#xAD50;&#xD558;&#xAE30;
          &#xC704;&#xD574; &#xCD08;&#xB9E4;&#xAC1C;&#xBCC0;&#xC218;&#xB97C; &#xC800;&#xC7A5;&#xD558;&#xB294;
          &#xAD6C;&#xC131;&#xC744; &#xC0AC;&#xC6A9;&#xD569;&#xB2C8;&#xB2E4;. ID&#xC5D0;
          &#xD2B9;&#xC218; &#xBB38;&#xC790;&#xB97C; &#xC0AC;&#xC6A9;&#xD560; &#xC218;
          &#xC5C6;&#xC2B5;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left">monitor_gym</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">OpenAI Gym&#xC758; &#xBE44;&#xB514;&#xC624; &#xB85C;&#xADF8;&#xD560;&#xC9C0;&#xC5D0;
        &#xB300;&#xD55C; &#xC5EC;&#xBD80;. &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
        <a
        href="https://docs.wandb.ai/v/ko/integrations/ray-tune">Ray Tune</a>&#xC744; &#xCC38;&#xC870;&#xD558;&#xC2DC;&#xAE30; &#xBC14;&#xB78D;&#xB2C8;&#xB2E4;
          (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
    <tr>
      <td style="text-align:left">allow_val_change</td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left">&#xD0A4;&#xB97C; &#xD55C;&#xBC88; &#xC124;&#xC815;&#xD55C; &#xD6C4;&#xC5D0;
        <a
        href="https://docs.wandb.com/library/config">&#xAD6C;&#xC131;</a>&#xAC12; &#xBCC0;&#xACBD; &#xD5C8;&#xC6A9; &#xC5EC;&#xBD80;.
          &#xAE30;&#xBCF8;&#xAC12;&#xC73C;&#xB85C;, &#xAD6C;&#xC131; &#xAC12;&#xC744;
          &#xB36E;&#xC5B4;&#xC4F0;&#xB294; &#xACBD;&#xC6B0; &#xC608;&#xC678;&#xAC00;
          &#xBC1C;&#xC0DD;&#xD569;&#xB2C8;&#xB2E4;. &#xD6C8;&#xB828; &#xC911; &#xC5EC;&#xB7EC;
          &#xBC88;&#xC5D0; &#xAC78;&#xCCD0; &#xB2E4;&#xC591;&#xD55C; learning_rate&#xC640;
          &#xAC19;&#xC740; &#xAC83;&#xB4E4;&#xC744; &#xCD94;&#xC801;&#xD558;&#xB824;&#xBA74;
          wandb.log()&#xB97C; &#xB300;&#xC2E0; &#xC0AC;&#xC6A9;&#xD558;&#xC2ED;&#xC2DC;&#xC624;.
          (&#xAE30;&#xBCF8;&#xAC12;: False)</td>
    </tr>
  </tbody>
</table>

이러한 설정의 대부분은 [환경 변수](https://docs.wandb.ai/v/ko/library/environment-variables)를 통해 제어할 수도 있습니다. 클러스터에서 작업을 실행할 때 유용합니다.

저희는 wandb.init\(\)를 실행하는 스크립트 복사본을 자동으로 저장합니다. 코드 비교 기능에 대한 자세한 사항은 [Code Comparer](https://docs.wandb.com/app/features/panels/code)에서 참조하시기 바랍니다. 이 기능을 비활성화하시려면, 환경 변수 WANDB\_DISABLE\_CODE=true를 설정합니다.

##  **공통 질문**

###   **한 스크립트에서 여러 실행을 진행하려면 어떻게 해야 합니까?**

 I하나의 스크립트에서 여러 실행을 시작하려는 경우, 다음 두 명령을 코드에 추가합니다:

1. run = wandb.init\(**reinit=True**\): 것을 사용해서 실행의 재 초기화를 허용합니다
2. **run.finish\(\)**: 실행이 끝날 때 이것을 사용해서 해당 실행에 대한 로그를 종료합니다.

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

또는 로그를 자동으로 종료하는 Python context manager를 사용할 수 있습니다:

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

### LaunchError: **허가되지 않음**

 **LaunchError: Launch exception: Permission denied \(권한 거부\)** 오류가 발생한 경우, 실행을 전송하려고 하는 프로젝트에 대한 로그 권한이 없음을 의미합니다. 여기에는 다음과 같은 여러 가지 이유가 있을 수 있습니다.

1. 이 머신에 로그인하지 않았습니다. 명령 줄에 `wandb login`을 실행합니다.
2.  존재하지 않는 개체를 설정했습니다. “Entity”\(개체\)는 여러분의 사용자 이름 또는 기존 팀의 이름이어야 합니다. 팀을 생성해야 하는 경우, [구독 페이지](https://app.wandb.ai/billing)로 이동하시기 바랍니다.
3.  프로젝트 로그 권한이 없습니다. 프로젝트에 실행을 로그할 수 있도록 프로젝트 생성자에게 개인 정보를 **Open\(공개\)**으로 설정하도록 문의하시기 바랍니다.

###  **실행 가능한 이름 가져오기**

실행 가능한 이름을 가져옵니다.

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

###  **생성된 실행 ID로 실행 이름 설정하기**

 실행 이름\(예: snowy-owl-10\)을 실행 ID \(예: qvlp96파\)로 덮어쓰려는 경우, 다음 스니펫\(snippet\)을 사용할 수 있습니다:

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

###  **git 커밋 저장**

wandb.init\(\)가 스크립트에서 호출되면, git 정보를 자동으로 검색하여 여러분의 repo, 가장 최근 커밋의 SHA에 대한 링크를 저장합니다. git 정보는 여러분의 [실행 페이지](https://docs.wandb.com/app/pages/run-page#overview-tab)에 표시됩니다. 실행 페이지에 나타나지 않는 경우 wandb.int\(\)를 호출한 스크립트가 git 정보가 있는 폴더에 있는지 확인하시기 바랍니다.  


 실험 실행에 사용된 git 커밋 및 명령은 사용자에게 표시되지만, 외부 사용자에게는 숨겨져 있으므로, 공개 프로젝트가 있는 경우, 이러한 상세 내용은 공개되지 않습니다.

###  **오프라인으로 로그 저장**

 기본값으로, wandb.init\(\)은 메트릭을 실시간으로 클라우드 호스팅 앱으로 동기화하는 프로세스를 시작합니다. 머신이 오프라인이거나 인터넷에 접속할 수 없는 경우, 다음과 같이 오프라인 모드를 사용해 wandb를 실행하고 나중에 동기화할 수 있습니다.

 ****다음의 두 가지 환경 변수를 설정합니다:

1. **WANDB\_API\_KEY**: [설정 페이지](https://app.wandb.ai/settings)의 계정 API 키에 이것을 설정합니다
2. **WANDB\_MODE**: dryrun

 스크립트에서 어떻게 표시되는지에 대한 샘플은 다음과 같습니다:

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

 터미널 출력 샘플은 다음과 같습니다:

![](../.gitbook/assets/image%20%2881%29.png)

그리고 인터넷에 연결이 되면, sync 명령을 실행하여 해당 폴더를 클라우드에 전송합니다.

`wandb sync wandb/dryrun-folder-name`

![](../.gitbook/assets/image%20%2836%29.png)

