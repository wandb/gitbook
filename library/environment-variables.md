# Environment Variables

当你在自动化环境中运行脚本时，你可以在脚本运行前或在脚本中设置环境变量来控制 **wandb** 。

```text
# This is secret and shouldn't be checked into version controlWANDB_API_KEY=$YOUR_API_KEY# Name and notes optionalWANDB_NAME="My first run"WANDB_NOTES="Smaller learning rate, more regularization."
```

```text
# Only needed if you don't checkin the wandb/settings fileWANDB_ENTITY=$usernameWANDB_PROJECT=$project
```

```text
# If you don't want your script to sync to the cloudos.environ['WANDB_MODE'] = 'dryrun'
```

### **可选环境变量**

使用这些可选环境变量来做诸如在远程机器上设置身份验证的事情。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53D8;&#x91CF;&#x540D;</th>
      <th style="text-align:left">&#x7528;&#x6CD5;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>WANDB_API_KEY</b>
      </td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x4E0E;&#x4F60;&#x7684;&#x8D26;&#x6237;&#x76F8;&#x5173;&#x8054;&#x7684;&#x8BA4;&#x8BC1;&#x5BC6;&#x5319;&#x3002;&#x4F60;&#x53EF;&#x4EE5;&#x5728;&#x4F60;&#x7684;
        <a
        href="https://app.wandb.ai/settings">&#x8BBE;&#x7F6E;&#x9875;&#x9762;</a>&#x627E;&#x5230;&#x4F60;&#x7684;&#x5BC6;&#x5319;&#xFF08;key&#xFF09;&#x3002;&#x5982;&#x679C;&#x8FD8;&#x672A;&#x5728;&#x8FDC;&#x7A0B;&#x673A;&#x5668;&#x4E0A;&#x8FD0;&#x884C;<code>wandb login</code> ,&#x5219;&#x5FC5;&#x987B;&#x8BBE;&#x7F6E;&#x8BE5;&#x53D8;&#x91CF;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_BASE_URL</b>
      </td>
      <td style="text-align:left">&#x5982;&#x679C;&#x4F60;&#x4F7F;&#x7528;&#x7684;&#x662F; <a href="https://docs.wandb.ai/self-hosted">wandb/local</a>&#xFF0C;&#x4F60;&#x5E94;&#x8BE5;&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;<code>http://&#x4F60;&#x7684;IP:&#x4F60;&#x7684;&#x7AEF;&#x53E3;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NAME</b>
      </td>
      <td style="text-align:left">&#x4F60;&#x7684;&#x8FD0;&#x884C;&#x7684;&#x53EF;&#x8BFB;&#x540D;&#x79F0;&#x3002;&#x5982;&#x679C;&#x6CA1;&#x6709;&#x8BBE;&#x7F6E;&#xFF0C;&#x5C06;&#x4E3A;&#x4F60;&#x968F;&#x673A;&#x751F;&#x6210;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NOTES</b>
      </td>
      <td style="text-align:left">&#x5173;&#x4E8E;&#x4F60;&#x7684;&#x8FD0;&#x884C;&#x7684;&#x66F4;&#x957F;&#x6CE8;&#x89E3;&#x3002;&#x5141;&#x8BB8;&#x4F7F;&#x7528;
        Markdown&#xFF0C;&#x4F60;&#x53EF;&#x4EE5;&#x7A0D;&#x540E;&#x5728;UI&#x4E2D;&#x7F16;&#x8F91;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ENTITY</b>
      </td>
      <td style="text-align:left">
        <p>&#x4E0E;&#x4F60;&#x7684;&#x8FD0;&#x884C;&#x76F8;&#x5173;&#x8054;&#x7684;&#x5B9E;&#x4F53;&#x3002;&#x5982;&#x679C;&#x4F60;&#x5728;&#x4F60;&#x7684;&#x8BAD;&#x7EC3;&#x811A;&#x672C;&#x7684;&#x76EE;&#x5F55;&#x4E0B;&#x8FD0;&#x884C;&#x4E86;<code>wandb init</code> &#xFF0C;&#x5B83;&#x5C06;&#x521B;&#x5EFA;&#x4E00;&#x4E2A;&#x540D;&#x4E3A;wandb</p>
        <p>&#x7684;&#x76EE;&#x5F55;&#xFF0C;&#x5E76;&#x5C06;&#x4FDD;&#x5B58;&#x4E00;&#x4E2A;&#x9ED8;&#x8BA4;&#x5B9E;&#x4F53;&#xFF0C;&#x5B83;&#x53EF;&#x4EE5;&#x68C0;&#x5165;&#x6E90;&#x4EE3;&#x7801;&#x63A7;&#x5236;&#x3002;&#x5982;&#x679C;&#x4F60;&#x4E0D;&#x60F3;&#x521B;&#x5EFA;&#x8FD9;&#x4E2A;&#x6587;&#x4EF6;&#x6216;&#x60F3;&#x8981;&#x8986;&#x76D6;&#x8FD9;&#x4E2A;&#x6587;&#x4EF6;&#xFF0C;&#x4F60;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x8BE5;&#x73AF;&#x5883;&#x53D8;&#x91CF;&#x3002;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_USERNAME</b>
      </td>
      <td style="text-align:left">&#x4E0E;&#x8BE5;&#x8FD0;&#x884C;&#x76F8;&#x5173;&#x8054;&#x7684;&#x4E00;&#x4E2A;&#x56E2;&#x961F;&#x6210;&#x5458;&#x7684;&#x7528;&#x6237;&#x540D;&#x3002;&#x8FD9;&#x53EF;&#x4EE5;&#x4E0E;&#x670D;&#x52A1;&#x8D26;&#x6237;API&#x5BC6;&#x5319;&#x4E00;&#x8D77;&#x4F7F;&#x7528;&#xFF0C;&#x4EE5;&#x4F7F;&#x81EA;&#x52A8;&#x8FD0;&#x884C;&#x5F52;&#x5C5E;&#x5230;&#x4F60;&#x7684;&#x56E2;&#x961F;&#x6210;&#x5458;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_PROJECT</b>
      </td>
      <td style="text-align:left">&#x4E0E;&#x4F60;&#x7684;&#x8FD0;&#x884C;&#x76F8;&#x5173;&#x8054;&#x7684;&#x9879;&#x76EE;&#x3002;&#x8FD9;&#x4E5F;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;<code>wandb init</code>&#x8BBE;&#x7F6E;&#xFF0C;&#x4F46;&#x662F;&#x8BE5;&#x73AF;&#x5883;&#x53D8;&#x91CF;&#x4F1A;&#x8986;&#x76D6;&#x5176;&#x503C;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_MODE</b>
      </td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#xFF0C;&#x8BE5;&#x53D8;&#x91CF;&#x88AB;&#x8BBE;&#x7F6E;&#x4E3A;<em>run</em> ,&#x4F1A;&#x5C06;&#x7ED3;&#x679C;&#x4FDD;&#x5B58;&#x5230;wandb&#x3002;&#x5982;&#x679C;&#x4F60;&#x53EA;&#x60F3;&#x628A;&#x8FD0;&#x884C;&#x7684;&#x5143;&#x6570;&#x636E;&#x4FDD;&#x5B58;&#x5728;&#x672C;&#x5730;&#xFF0C;&#x4F60;&#x53EF;&#x4EE5;&#x628A;&#x5B83;&#x8BBE;&#x7F6E;&#x4E3A; <em>dryrun &#x3002;</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_TAGS</b>
      </td>
      <td style="text-align:left">&#x8981;&#x5E94;&#x7528;&#x4E8E;&#x8FD0;&#x884C;&#x7684;&#x4E00;&#x4E2A;&#x7528;&#x9017;&#x53F7;&#x5206;&#x5272;&#x7684;&#x6807;&#x7B7E;&#x5217;&#x8868;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DIR</b>
      </td>
      <td style="text-align:left">&#x5B58;&#x50A8;&#x6240;&#x6709;&#x751F;&#x6210;&#x6587;&#x4EF6;&#x7684;&#x7EDD;&#x5BF9;&#x8DEF;&#x5F84;&#xFF0C;&#x4EE3;&#x66FF;&#x9ED8;&#x8BA4;&#x7684;&#x76F8;&#x5BF9;&#x4E8E;&#x4F60;&#x7684;&#x811A;&#x672C;&#x7684;<em>wandb</em> &#x8DEF;&#x5F84;&#x3002;&#x786E;&#x4FDD;&#x8BE5;&#x8DEF;&#x5F84;&#x5B58;&#x5728;&#xFF0C;&#x5E76;&#x4E14;&#x8FD0;&#x884C;&#x4F60;&#x7684;&#x8FDB;&#x7A0B;&#x7684;&#x7528;&#x6237;&#x6709;&#x5199;&#x5165;&#x6743;&#x9650;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RESUME</b>
      </td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#x8BE5;&#x53D8;&#x91CF;&#x88AB;&#x8BBE;&#x7F6E;&#x4E3A; <em>never</em>.
        &#x5982;&#x679C;&#x8BBE;&#x7F6E;&#x4E3A; <em>auto</em> wandb &#x5C06;&#x81EA;&#x52A8;&#x6062;&#x590D;&#x5931;&#x8D25;&#x7684;&#x8FD0;&#x884C;&#x3002;&#x5982;&#x679C;&#x8BBE;&#x7F6E;&#x4E3A; <em>must</em>&#xFF0C;&#x5219;&#x5F3A;&#x5236;&#x8BE5;&#x8FD0;&#x884C;&#x5728;&#x542F;&#x52A8;&#x65F6;&#x5B58;&#x5728;&#x3002;&#x5982;&#x679C;&#x4F60;&#x60F3;&#x603B;&#x662F;&#x751F;&#x6210;&#x4F60;&#x81EA;&#x5DF1;&#x7684;&#x552F;&#x4E00;ID,&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A; <em>allow</em> &#x5E76;&#x603B;&#x662F;&#x8BBE;&#x7F6E; <b>WANDB_RUN_ID&#x3002;</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RUN_ID</b>
      </td>
      <td style="text-align:left">&#x5BF9;&#x5E94;&#x4E8E;&#x4F60;&#x7684;&#x811A;&#x672C;&#x7684;&#x5355;&#x6B21;&#x8FD0;&#x884C;&#xFF0C;&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;&#x4E00;&#x4E2A;&#x5168;&#x5C40;&#x552F;&#x4E00;&#x5B57;&#x7B26;&#x4E32;&#xFF08;&#x6BCF;&#x4E2A;&#x9879;&#x76EE;&#xFF09;&#x3002;&#x5B83;&#x5FC5;&#x987B;&#x4E0D;&#x957F;&#x4E8E;64&#x4E2A;&#x5B57;&#x7B26;&#x3002;&#x6240;&#x6709;&#x975E;&#x5355;&#x8BCD;&#x5B57;&#x7B26;&#x5C06;&#x88AB;&#x8F6C;&#x6362;&#x4E3A;&#x8FDE;&#x5B57;&#x7B26;&#x3002;&#x8FD9;&#x53EF;&#x4EE5;&#x7528;&#x6765;&#x6062;&#x590D;&#x5DF2;&#x7ECF;&#x5B58;&#x5728;&#x7684;&#x5931;&#x8D25;&#x8FD0;&#x884C;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_IGNORE_GLOBS</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x6B64;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;&#x8981;&#x5FFD;&#x7565;&#x7684;&#x6587;&#x4EF6;&#x901A;&#x914D;&#x7B26;&#x5217;&#x8868;&#xFF0C;&#x4EE5;&#x9017;&#x53F7;&#x5206;&#x5272;&#x3002;&#x8FD9;&#x4E9B;&#x6587;&#x4EF6;&#x5C06;&#x4E0D;&#x4F1A;&#x88AB;&#x540C;&#x6B65;&#x5230;&#x4E91;&#x7AEF;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ERROR_REPORTING</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A; false,&#x4EE5;&#x9632;&#x6B62;wandb&#x5C06;&#x81F4;&#x547D;&#x9519;&#x8BEF;&#x8BB0;&#x5F55;&#x5230;&#x5176;&#x9519;&#x8BEF;&#x8DDF;&#x8E2A;&#x7CFB;&#x7EDF;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_SHOW_RUN</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;<b>true</b>&#xFF0C;&#x5982;&#x679C;&#x4F60;&#x7684;&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#x652F;&#x6301;&#xFF0C;&#x5C06;&#x5728;&#x6D4F;&#x89C8;&#x5668;&#x4E2D;&#x81EA;&#x52A8;&#x6253;&#x5F00;&#x8FD0;&#x884C;URL.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DOCKER</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;&#x4E00;&#x4E2A;docker&#x955C;&#x50CF;&#x6458;&#x8981;&#x4EE5;&#x4FBF;&#x80FD;&#x591F;&#x6062;&#x590D;&#x8FD0;&#x884C;&#x3002;&#x8FD9;&#x662F;&#x901A;&#x8FC7;wandb
        docker &#x547D;&#x4EE4;&#x81EA;&#x52A8;&#x8BBE;&#x7F6E;&#x7684;&#x3002;&#x4F60;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;&#x8FD0;&#x884C;<code>wandb docker my/image/name:tag --digest</code>&#x83B7;&#x53D6;&#x4E00;&#x4E2A;&#x955C;&#x50CF;&#x7684;&#x6458;&#x8981;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_DISABLE_CODE</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;true &#x4EE5;&#x9632;&#x6B62;wandb&#x5B58;&#x50A8;&#x5BF9;&#x4F60;&#x7684;&#x6E90;&#x4EE3;&#x7801;&#x7684;&#x5F15;&#x7528;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_ANONYMOUS</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A; &quot;allow&quot;,
        &quot;never&quot;, &#x6216;&quot;must&quot; &#x4EE5;&#x4FBF;&#x8BA9;&#x7528;&#x6237;&#x4F7F;&#x7528;&#x79D8;&#x5BC6;URL&#x521B;&#x5EFA;&#x533F;&#x540D;&#x8FD0;&#x884C;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONSOLE</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x8BE5;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;&quot;off&quot;
        &#x4EE5;&#x7981;&#x7528; &#x6807;&#x51C6;&#x8F93;&#x51FA;&#xFF08;stdout&#xFF09;/
        &#x6807;&#x51C6;&#x9519;&#x8BEF;&#xFF08;stderr &#xFF09;&#x8BB0;&#x5F55;&#x3002;&#x5728;&#x652F;&#x6301;&#x8BE5;&#x529F;&#x80FD;&#x7684;&#x73AF;&#x5883;&#x4E2D;&#x9ED8;&#x8BA4;&#x4E3A;&quot;on&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONFIG_PATHS</b>
      </td>
      <td style="text-align:left">&#x8981;&#x52A0;&#x8F7D;&#x5230;wandb.config&#x4E2D;&#x7684;&#x4EE5;&#x9017;&#x53F7;&#x5206;&#x5272;&#x7684;yaml
        &#x6587;&#x4EF6;&#x5217;&#x8868;&#x3002;&#x53C2;&#x89C1; <a href="https://docs.wandb.ai/library/config#file-based-configs">config</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_CONFIG_DIR</b>
      </td>
      <td style="text-align:left">&#x8BE5;&#x53D8;&#x91CF;&#x7684;&#x9ED8;&#x8BA4;&#x503C;&#x4E3A; ~/.config/wandb&#xFF0C;&#x4F60;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;&#x8BE5;&#x73AF;&#x5883;&#x53D8;&#x91CF;&#x8986;&#x76D6;&#x9ED8;&#x8BA4;&#x503C;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_NOTEBOOK_NAME</b>
      </td>
      <td style="text-align:left">&#x5982;&#x679C;&#x4F60;&#x5728;jupyter &#x4E2D;&#x8FD0;&#x884C;&#xFF0C;&#x4F60;&#x53EF;&#x4EE5;&#x7528;&#x8FD9;&#x4E2A;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x7B14;&#x8BB0;&#x672C;&#xFF08;notebook&#xFF09;&#x7684;&#x540D;&#x79F0;&#x3002;&#x6211;&#x4EEC;&#x4F1A;&#x5C1D;&#x8BD5;&#x81EA;&#x52A8;&#x76D1;&#x6D4B;&#x5B83;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_HOST</b>
      </td>
      <td style="text-align:left">&#x5982;&#x679C;&#x4F60;&#x4E0D;&#x60F3;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;&#x63D0;&#x4F9B;&#x7684;&#x4E3B;&#x673A;&#x540D;&#xFF08;hostname&#xFF09;&#x8BF7;&#x5C06;&#x6B64;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;&#x4F60;&#x5E0C;&#x671B;&#x5728;wandb&#x754C;&#x9762;&#x770B;&#x5230;&#x7684;&#x4E3B;&#x673A;&#x540D;&#xFF08;hostname&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_SILENT</b>
      </td>
      <td style="text-align:left">&#x5C06;&#x6B64;&#x53D8;&#x91CF;&#x8BBE;&#x7F6E;&#x4E3A;<b>true</b>&#x53EF;&#x4EE5;&#x4F7F;wandb&#x8BED;&#x53E5;&#x9759;&#x9ED8;&#x3002;&#x5982;&#x679C;&#x8BBE;&#x7F6E;&#x4E86;,&#x6240;&#x6709;&#x65E5;&#x5FD7;&#x90FD;&#x4F1A;&#x88AB;&#x5199;&#x5165;&#x5230;<b>WANDB_DIR</b>/debug.log</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_RUN_GROUP</b>
      </td>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x5B9E;&#x9A8C;&#x540D;&#x79F0;&#x4EE5;&#x81EA;&#x52A8;&#x5C06;&#x8FD0;&#x884C;&#x5206;&#x7EC4;&#x3002;&#x66F4;&#x591A;&#x4FE1;&#x606F;&#x8BF7;&#x53C2;&#x89C1;
        <a
        href="https://docs.wandb.ai/library/grouping">&#x5206;&#x7EC4;</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WANDB_JOB_TYPE</b>
      </td>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x4F5C;&#x4E1A;&#x7C7B;&#x578B;&#xFF0C;&#x5982;&#x2018;&#x8BAD;&#x7EC3;&#x2019;&#x6216;&#x2018;&#x8BC4;&#x4F30;&#x2019;&#xFF0C;&#x4EE5;&#x8868;&#x793A;&#x4E0D;&#x540C;&#x7C7B;&#x578B;&#x7684;&#x8FD0;&#x884C;&#x3002;&#x66F4;&#x591A;&#x4FE1;&#x606F;&#x8BF7;&#x53C2;&#x89C1;
        <a
        href="https://docs.wandb.ai/library/grouping">&#x5206;&#x7EC4;</a>
      </td>
    </tr>
  </tbody>
</table>

## **Singularity环境** <a id="singularity-environments"></a>

如果你在[Singularity](https://singularity.lbl.gov/index.html) 环境中运行容器，你可以通过在上述变量前加上**SINGULARITYENV\_**来传递变量。关于Singularity 环境变量的更多细节可以在[这里](https://singularity.lbl.gov/docs-environment-metadata#environment)找到。

## **在AWS上运行** <a id="running-on-aws"></a>

如果你在AWS中运行批处理作业，用你的W&B凭证来验证你的机器会非常容易。从你的[设置页面](https://app.wandb.ai/settings)获取你的API密匙，并在[AWS批处理作业规范](https://docs.aws.amazon.com/batch/latest/userguide/job_definition_parameters.html#parameters)中设置WANDB\_API\_KEY环境变量。 

##  **常见问题** <a id="common-questions"></a>

###  **自动运行和服务账户** <a id="automated-runs-and-service-accounts"></a>

如果你有自动测试或内部工具启动运行记录到W&B,请在你的团队设置页面创建一个**服务账户。**这允许你为你的自动化作业使用服务API密匙。如果你想把服务账户的作业归属于一个特定用户，你可以使用WANDB\_USER\_NAME 或WANDB\_USER\_EMAIL 环境变量。environment variables.![](https://gblobscdn.gitbook.com/assets%2F-Lqya5RvLedGEWPhtkjU%2F-MCOHlPPZL6wnVHQJ4MO%2F-MCOJERUiOrCn4y4MjZn%2Fimage.png?alt=media&token=e4e7f94c-cddd-47ce-b678-c7ecc8ed369d)在你的团队设置页面为你的自动化作业设置一个服务账号。

如果你正在设置自动化单元测试的话，这对于持续集成和TravisCI或 CircleCI 等工具来说很有用。

### **环境变量是否会覆盖传递给wandb.init\(\)的参数?**

传递给`wandb.init`的参数优先级高于环境变量。如果你想在没有设置环境变量的情况下使用系统默认值以外的默认值，你可以调用 `wandb.init(dir=os.getenv("WANDB_DIR", my_default_override))` 。

###  **关闭记录** <a id="turn-off-logging"></a>

 命令 `wandb off` 设置了一个环境变量, `WANDB_MODE=dryrun` 。这将停止任何数据从你的机器同步到远程wandb服务器。你有多个项目，它们都将停止将记录的数据同步到W&B服务器。

 要使警告消息静音：

```text
import logginglogger = logging.getLogger("wandb")logger.setLevel(logging.WARNING)
```

###  **共享机器上有多个wandb用户** <a id="multiple-wandb-users-on-shared-machines"></a>

如果你使用的是一个共享机器，而另一个用户是wandb用户，确保你的运行登录到正确的账户很容易。设置[WANDB\_API\_KEY环](https://docs.wandb.ai/library/environment-variables)境变量 来验证。如果你在你的环境中source 它，当你登录时，你将有正确的凭证。 或者你也可以从你的脚本中设置环境变量。

运行命令 `export WANDB_API_KEY=X` 其中X是你的API密匙。当你登录后，你可以在[wandb.ai/authorize](https://app.wandb.ai/authorize) 找到你的API密匙。

