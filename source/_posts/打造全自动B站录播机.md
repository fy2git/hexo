---
abbrlink: ''
categories:
- - 教程
date: '2023-04-12T14:17:54.740008+08:00'
tags:
- B站
title: 打造全自动B站录播机
updated: 2023-4-12T14:17:55.362+8:0
---
## 前言

**随着B站虚拟主播越来越多，很多主播/粉丝都有录播的需求。当然不是粉丝也可以做录播。现在录播基本上都是在服务器上实时监测，监测到主播开播之后就会自动开始录播。就我个人而言，录播可能是防止错过关注的主播直播，或者是没时间看，就看看录播。还有就是看看有没有好玩的地方可以做成切片素材，这样可以做成切片上传到B站，也可以获得一定的收益（聊胜于无，但可以用来支付服务器开销）。**

**这种全自动的录播机也可以帮你赚到钱，前提是你自己能去主动找主播推荐自己的切片服务。你可以收取技术指导费，然后教主播怎么弄。也可以直接提供全自动录播上传一条龙服务，然后主播按月付费这种形式。推荐找几万粉的小虚拟主播谈这个，因为粉丝多了自然会有人自发去做这个。几万粉的小主播有一定的直播收入来源，忠实粉丝相对较少，不一定有能且愿意的人。商业化的方向请自行探索。这里提到的只供参考。**

## 工具选择

**我这里介绍两个工具，他们各自都有各自的特点。看你自己喜欢哪一个**

### 1、BililiveRecorder 又名B站录播姬

**源码：**[BililiveRecorder](https://github.com/BililiveRecorder/BililiveRecorder)

**特点：**

* **使用简单**
* **主播开播后自动开始录制**
* **同时录制多个直播间**
* **自动修复B站直播服务器导致的各种问题**
* **工具箱模式，用于修复旧版录播姬或其他软件录的视频文件**
* **纯 C# 实现，无 ffmpeg 等 native 依赖**

**拥有图形化界面，操作简单。可以快速添加房号，可以在线播放，存储弹幕信息。支持webhook**

**缺点：无法进行投稿。可以使用其他B站投稿程序进行投稿。如****[auto-bilibili-recorder](https://github.com/valkjsaaa/auto-bilibili-recorder)**

**界面：**

![](https://image.xuehaiwu.com/2023/03/31/chrome_0y3uOMCVlM.png)

**webui**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**房间列表**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**文件管理器**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**设置**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**日志**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**file/文件浏览器**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**播放页：**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

### 2、biliup

* **支持自动录制各大主流直播平台实时直播流，包括但不限于acfun，afreecaTV，哔哩哔哩，斗鱼，抖音，虎牙，网易CC，猫耳FM，Twitch，YY直播等，并于录制结束后上传到哔哩哔哩视频网站。**
* **支持YouTube，twitch直播回放列表自动搬运至b站，如链接**[https://www.twitch.tv/xxxx/videos?filter=archives&sort=time](https://www.twitch.tv/xxxx/videos?filter=archives&sort=time)
* **自动选择上传线路，保证国内外vps上传质量和速度**
* **可分别控制下载与上传并发量**
* **支持 cos-internal，腾讯云上海内网上传，免流 + 大幅提速**
* **实验性功能：**
  * **防止录制花屏**
  * **启动时加入**`--http`选项并访问localhost:19159可使用webUI (建议使用toml配置文件)

**缺点就是配置较为繁琐，其他都还行。**

**请根据自己的需求选择上面的程序。**

## 教程

### 1、服务器选择与购买

**服务器我选择的是腾讯云的轻量云，地域是上海。主要是B站上传投稿有一条线路是COS内网。选择上海的轻量服务器，可以直接走内网上传到COS，速度肯定是拉满的。其次是B站的直播分发主要还是国内，所以能选国内服务器尽量还是选择国内服务器。**

**轻量服务器应用专场：**[活动地址](https://url.cn/tL37ikXK)

#### ![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**最低配置：2C2G4M 50GB SSD**

**推荐配置：4C8G12M 180GB SSD**

**推荐选择4C8G12M的服务器。主要是硬盘不能太小，不然录制原画画质的情况下15分钟左右文件大小就有1GB了，遇上高强度直播的主播，硬盘太小可能撑不到一场直播结束就塞满了。**

**正常主播推流的画质大约在3-8000的码率，可以根据你需要录播的主播平常时长和推流码率来选择服务器硬盘大小。如果你有在线播放的需求，还得看服务器带宽，最好不低于6M,推荐8M及以上。**

**另外就是如果有长时间存储的需求，可以考虑购买COS存储资源包，和额外的云硬盘。**

**你可以挂载额外的云硬盘来同时录播多位主播，根据你自己的业务需求来就行。然后设置个定时脚本每天白天或者凌晨上传前一天的录播文件到COS桶里。（不推荐直接设置挂载COS桶的路径为录播文件存储路径。会造成大量的读写操作还容易损坏文件。）**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**地域一定要选择上海，镜像选择宝塔面板即可，后面也可以在控制台进行更换。**

**云硬盘设置：**直接轻量服务器控制台——云硬盘——地域（上海）——更多——挂载到服务器即可

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**初始化云硬盘可以参照**[腾讯云官方文档](https://cloud.tencent.com/document/product/1207/63943)进行操作。

**服务器防火墙：**可以直接开放全部端口，也可以开放指定端口如2356.

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**如果用的宝塔面板，后续也需要在面板的安全选项里进行开发端口**

### 2、软件安装及使用

#### 2.1 B站录播姬安装及使用

**项目地址：**[https://github.com/BililiveRecorder/BililiveRecorder](https://github.com/BililiveRecorder/BililiveRecorder)

##### 2.1.1 安装

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">mkdir brec</span></span></p></li><li class="L1" data-node-id="20230412141928-ezfe8bj"><p><span role="presentation"><span class="pln">cd brec</span></span></p></li><li class="L2"><p><span role="presentation"><span class="com"># wget 下载链接</span></span></p></li><li class="L3" data-node-id="20230412141928-tqbaaxf"><p><span role="presentation"><span class="pln">unzip </span><span class="typ">BililiveRecorder</span><span class="pun">-</span><span class="pln">CLI</span><span class="pun">-</span><span class="pln">linux</span><span class="pun">-</span><span class="pln">x64</span><span class="pun">.</span><span class="pln">zip</span></span></p></li></ol></pre>

**进入GitHub release页面下载最新的linux x64命令行版本获取下载链接，国内服务器可能与github连通性不好，可以使用加速连接进行下载**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="typ">Github</span><span class="pun">:</span></span></p></li><li class="L1" data-node-id="20230412141928-jhojbpv"><p><span role="presentation"><span class="pln">https</span><span class="pun">:</span><span class="com">//github.com/BililiveRecorder/BililiveRecorder/releases/latest/download/BililiveRecorder-CLI-linux-x64.zip</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pun">加速连接：</span></span></p></li><li class="L3" data-node-id="20230412141928-vgjwsc2"><p><span role="presentation"><span class="pln">https</span><span class="pun">:</span><span class="com">//ghproxy.com/https://github.com/BililiveRecorder/BililiveRecorder/releases/latest/download/BililiveRecorder-CLI-linux-x64.zip</span></span></p></li></ol></pre>

**添加执行权限**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">chmod </span><span class="pun">+</span><span class="pln">x </span><span class="typ">BililiveRecorder</span><span class="pun">.</span><span class="typ">Cli</span></span></p></li></ol></pre>

**检查版本号**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pun">./</span><span class="typ">BililiveRecorder</span><span class="pun">.</span><span class="typ">Cli</span><span class="pln"> </span><span class="pun">--</span><span class="pln">version</span></span></p></li></ol></pre>

**为了后续方便可以修改**`BililiveRecorder.Cli` 为 `brec`

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">mv </span><span class="typ">BililiveRecorder</span><span class="pun">.</span><span class="typ">Cli</span><span class="pln"> brec</span></span></p></li></ol></pre>

##### 2.1.2 使用

**可以先创建一个工作目录，第一运行程序会自动生成一个配置文件。然后重新启动程序即可这里以**`file`为工作目录。Ctrl+C退出

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">mkdir file</span></span></p></li><li class="L1" data-node-id="20230412141928-jd4j32o"><p><span role="presentation"><span class="pun">./</span><span class="pln">brec run file</span></span></p></li></ol></pre>

**录播姬命令行版提供了 HTTP API 和管理网页，可以通过 **`--bind` 参数启用。需要防火墙和宝塔都开放2356端口

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="com"># 侦听本机地址，只有本地可以访问</span></span></p></li><li class="L1" data-node-id="20230412141928-ya95q9d"><p><span role="presentation"><span class="pun">./</span><span class="pln">brec run </span><span class="pun">--</span><span class="pln">bind </span><span class="str">"http://localhost:2356"</span><span class="pln"> </span><span class="str">"工作目录"</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L3" data-node-id="20230412141928-r470rk2"><p><span role="presentation"><span class="com"># 或者所有设备都可访问，开放至公网推荐开启登陆认证</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pun">./</span><span class="pln">brec run </span><span class="pun">--</span><span class="pln">bind </span><span class="str">"http://*:2356"</span><span class="pln"> </span><span class="str">"工作目录"</span></span></p></li></ol></pre>

**登录认证**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pun">.</span><span class="str">/brec run --bind "http:/</span><span class="com">/*:2356" --http-basic-user "用户名" --http-basic-pass "密码" "工作目录"</span></span></p></li></ol></pre>

**由于录播无所谓其他用户，基本上只有自己在用，所以基本不需要用域名或者https。如果你需要的话可以直接通过宝塔自带的反向代理功能实现。**

##### 2.1.3 持久化运行

**创建服务：**

**新建一个文件：**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">nano </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">systemd</span><span class="pun">/</span><span class="pln">system</span><span class="pun">/</span><span class="pln">brec</span><span class="pun">.</span><span class="pln">service</span></span></p></li></ol></pre>

**写入以下内容：注意根据你自己的实际情况调整**`ExecStart=`后的文件路径和参数 nano编辑器Ctrl+O为保存，Ctrl+X为退出

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pun">[</span><span class="typ">Unit</span><span class="pun">]</span></span></p></li><li class="L1" data-node-id="20230412141928-vv8v6kq"><p><span role="presentation"><span class="typ">Description</span><span class="pun">=</span><span class="typ">BililiveRecorder</span></span></p></li><li class="L2"><p><span role="presentation"><span class="typ">After</span><span class="pun">=</span><span class="pln">network</span><span class="pun">.</span><span class="pln">target</span></span></p></li><li class="L3" data-node-id="20230412141928-2vykpo9"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L4"><p><span role="presentation"><span class="pun">[</span><span class="typ">Service</span><span class="pun">]</span></span></p></li><li class="L5" data-node-id="20230412141928-9pm92sl"><p><span role="presentation"><span class="typ">ExecStart</span><span class="pun">=录播姬所在位置/</span><span class="typ">BililiveRecorder</span><span class="pun">.</span><span class="typ">Cli</span><span class="pln"> run </span><span class="pun">--</span><span class="pln">bind </span><span class="str">"http://*:2356"</span><span class="pln"> </span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">basic</span><span class="pun">-</span><span class="pln">user </span><span class="str">"用户名"</span><span class="pln"> </span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">basic</span><span class="pun">-</span><span class="kwd">pass</span><span class="pln"> </span><span class="str">"密码"</span><span class="pln"> </span><span class="str">"录播工作目录"</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L7" data-node-id="20230412141928-nsirkqs"><p><span role="presentation"><span class="pun">[</span><span class="typ">Install</span><span class="pun">]</span></span></p></li><li class="L8"><p><span role="presentation"><span class="typ">WantedBy</span><span class="pun">=</span><span class="pln">multi</span><span class="pun">-</span><span class="pln">user</span><span class="pun">.</span><span class="pln">target</span></span></p></li></ol></pre>

**然后重启服务：**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">systemctl daemon</span><span class="pun">-</span><span class="pln">reload</span></span></p></li></ol></pre>

**每次修改配置都要重启一次。**

**启动录播姬**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">systemctl start brec</span></span></p></li></ol></pre>

**设置开机启动**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">systemctl enable brec</span></span></p></li><li class="L1" data-node-id="20230412141928-7swetux"><p><span role="presentation"><span class="pln">systemctl disable brec </span><span class="com">#禁用开机启动</span></span></p></li></ol></pre>

**其他命令**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="com">#查看运行状态</span></span></p></li><li class="L1" data-node-id="20230412141928-egond26"><p><span role="presentation"><span class="pln">systemctl status brec</span></span></p></li><li class="L2"><p><span role="presentation"><span class="com">#运行、停止、重启</span></span></p></li><li class="L3" data-node-id="20230412141928-0wln14j"><p><span role="presentation"><span class="pln">systemctl start brec</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">systemctl stop brec</span></span></p></li><li class="L5" data-node-id="20230412141928-ho141vo"><p><span role="presentation"><span class="pln">systemctl restart brec</span></span></p></li><li class="L6"><p><span role="presentation"><span class="com">#查看日志</span></span></p></li><li class="L7" data-node-id="20230412141928-5lgebr3"><p><span role="presentation"><span class="pln">journalctl </span><span class="pun">-</span><span class="pln">u brec</span><span class="pun">.</span><span class="pln">service</span></span></p></li></ol></pre>

##### 2.1.4 软件设置

**进入webUI页面，使用方法也很简单**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**房间号可以直接填数字，也可以直接复制链接**

**比如**[https://live.bilibili.com/6](https://live.bilibili.com/6) 是LPL的赛事直播间，可以填6也可以全部填上去。

**针对单一主播可以单独设置**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**全局设置：**

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

![](https://www.xuehaiwu.com/wp-content/themes/b2/Assets/fontend/images/default-img.jpg)

**如果需要上传投稿，可以搭配**[biliup-rs](https://github.com/ForgQi/biliup-rs)。这是下面我们要介绍的biliup的命令行版投稿工具。可以参考下面的教程。

#### 2.2 biliup的安装及使用

**项目地址：**[https://github.com/biliup/biliup](https://github.com/biliup/biliup)

##### 2.2.1 安装

**不推荐使用centos，如果搭建这个项目建议还是更换系统为Debian或者Ubuntu**

**Debian、Ubuntu**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pun">第一步：安装</span><span class="pln">python3</span><span class="pun">-</span><span class="pln">dev</span></span></p></li><li class="L1" data-node-id="20230412141928-a0yacll"><p><span role="presentation"><span class="pln">apt install python3</span><span class="pun">-</span><span class="pln">dev</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">apt install python3</span><span class="pun">-</span><span class="pln">pip</span></span></p></li><li class="L3" data-node-id="20230412141928-ufyceb1"><p><span role="presentation"><span class="pun">第二步：安装</span><span class="pln">ffmpeg</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">apt install ffmpeg</span></span></p></li><li class="L5" data-node-id="20230412141928-lzo3wsm"><p><span role="presentation"><span class="pun">第三步：安装</span><span class="pln">nodejs</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pln">apt install nodejs</span></span></p></li><li class="L7" data-node-id="20230412141928-vyhmbn8"><p><span role="presentation"><span class="pun">第四步：安装</span><span class="pln">biliup</span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">pip3 install biliup</span></span></p></li></ol></pre>

##### 2.2.2 使用

**首先要创建配置文件**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">mkdir biliup</span></span></p></li><li class="L1" data-node-id="20230412141928-hm9t39p"><p><span role="presentation"><span class="pln">cd biliup</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">nano config</span><span class="pun">.</span><span class="pln">yaml</span></span></p></li></ol></pre>

**配置文件内容根据自己情况进行修改**

**最小配置示例：**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="pln">user</span><span class="pun">:</span><span class="pln"> </span></span></p></li><li class="L1" data-node-id="20230412141928-gclag2j"><p><span role="presentation"><span class="pln">    cookies</span><span class="pun">:</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">        SESSDATA</span><span class="pun">:</span><span class="pln"> your SESSDATA</span></span></p></li><li class="L3" data-node-id="20230412141928-5div8ae"><p><span role="presentation"><span class="pln">        bili_jct</span><span class="pun">:</span><span class="pln"> your bili_jct</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">        </span><span class="typ">DedeUserID__ckMd5</span><span class="pun">:</span><span class="pln"> your ckMd5</span></span></p></li><li class="L5" data-node-id="20230412141928-0ai4b0e"><p><span role="presentation"><span class="pln">        </span><span class="typ">DedeUserID</span><span class="pun">:</span><span class="pln"> your </span><span class="typ">DedeUserID</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pln">    access_token</span><span class="pun">:</span><span class="pln"> your access_key</span></span></p></li><li class="L7" data-node-id="20230412141928-pkzj2rt"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">streamers</span><span class="pun">:</span></span></p></li><li class="L9" data-node-id="20230412141928-1x6l9c6"><p><span role="presentation"><span class="pln">    xxx</span><span class="pun">直播录像:</span><span class="pln"> </span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">        url</span><span class="pun">:</span></span></p></li><li class="L1" data-node-id="20230412141928-609otp4"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> https</span><span class="pun">:</span><span class="com">//www.twitch.tv/xxx</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">        tags</span><span class="pun">:</span><span class="pln"> biliup</span></span></p></li></ol></pre>

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="com">#config.yaml配置文件示例</span></span></p></li><li class="L1" data-node-id="20230412141928-fkq6ck1"><p><span role="presentation"><span class="pln">user</span><span class="pun">:</span><span class="pln"> </span><span class="com"># 在填了cookies的情况下优先使用cookies上传，如需使用用户名密码上传请注释掉cookies</span></span></p></li><li class="L2"><p><span role="presentation"><span class="com">#    cookies:</span></span></p></li><li class="L3" data-node-id="20230412141928-ir1mlyt"><p><span role="presentation"><span class="com">#        SESSDATA: your SESSDATA</span></span></p></li><li class="L4"><p><span role="presentation"><span class="com">#        bili_jct: your bili_jct</span></span></p></li><li class="L5" data-node-id="20230412141928-qjtajf2"><p><span role="presentation"><span class="com">#        DedeUserID__ckMd5: your ckMd5</span></span></p></li><li class="L6"><p><span role="presentation"><span class="com">#        DedeUserID: your DedeUserID</span></span></p></li><li class="L7" data-node-id="20230412141928-neyow7u"><p><span role="presentation"><span class="com">#    access_token: your access_key</span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">    account</span><span class="pun">:</span></span></p></li><li class="L9" data-node-id="20230412141928-7fs133w"><p><span role="presentation"><span class="pln">        username</span><span class="pun">:</span><span class="pln"> your usrname</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">        password</span><span class="pun">:</span><span class="pln"> your password</span></span></p></li><li class="L1" data-node-id="20230412141928-t02fabf"><p><span role="presentation"><span class="com">#    app_key: bca7e84c2d947ac6 # 若账号密码方式无法登录可尝试更改此值</span></span></p></li><li class="L2"><p><span role="presentation"><span class="com">#    appsec: 60698ba2f68e01ce44738920a0ffe768 # 值可以参考 https://github.com/SocialSisterYi/bilibili-API-collect/blob/master/other/API_auth.md</span></span></p></li><li class="L3" data-node-id="20230412141928-j5yjurk"><p><span class="pln"> </span></p></li><li class="L4"><p><span role="presentation"><span class="com"># b站上传线路选择，默认为自动模式，目前可手动切换为bda2, kodo, ws, qn</span></span></p></li><li class="L5" data-node-id="20230412141928-z1rn0yk"><p><span role="presentation"><span class="pln">lines</span><span class="pun">:</span><span class="pln"> AUTO</span></span></p></li><li class="L6"><p><span role="presentation"><span class="com"># b站提交接口，默认自动选择，可选web，client</span></span></p></li><li class="L7" data-node-id="20230412141928-5llv2wg"><p><span role="presentation"><span class="com">#submit_api: client</span></span></p></li><li class="L8"><p><span role="presentation"><span class="com"># 单文件并发上传数，未达到带宽上限时增大此值可提高上传速度</span></span></p></li><li class="L9" data-node-id="20230412141928-4lbgj60"><p><span role="presentation"><span class="pln">threads</span><span class="pun">:</span><span class="pln"> </span><span class="lit">3</span></span></p></li><li class="L0"><p><span role="presentation"><span class="com"># 录像单文件大小限制，单位Byte，超过此大小分段下载</span></span></p></li><li class="L1" data-node-id="20230412141928-dkb6926"><p><span role="presentation"><span class="pln">file_size</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2621440000</span></span></p></li><li class="L2"><p><span role="presentation"><span class="com"># 录像单文件时间限制，格式'00:00:00'（时分秒），超过此大小分段下载，如需使用大小分段请注释此字段</span></span></p></li><li class="L3" data-node-id="20230412141928-gqanrks"><p><span role="presentation"><span class="com">#segment_time: '00:50:00'</span></span></p></li><li class="L4"><p><span role="presentation"><span class="com">#douyucdn: tct-h5</span></span></p></li><li class="L5" data-node-id="20230412141928-fh9a1dr"><p><span role="presentation"><span class="com"># 如遇到斗鱼录制卡顿可以尝试切换线路，tct-h5（备用线路5），ali-h5（备用线路6），akm-h5（主线路1）</span></span></p></li><li class="L6"><p><span role="presentation"><span class="com">#huyacdn: AL</span></span></p></li><li class="L7" data-node-id="20230412141928-o6sn55g"><p><span role="presentation"><span class="com"># 如遇到虎牙录制卡顿可以尝试切换线路，AL（阿里），BD（百度），TX（腾讯）</span></span></p></li><li class="L8"><p><span class="pln"> </span></p></li><li class="L9" data-node-id="20230412141928-tazb85a"><p><span role="presentation"><span class="pln">streamers</span><span class="pun">:</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">    </span><span class="pun">星际</span><span class="lit">2Stats</span><span class="pun">拔本神族天梯第一视角:</span><span class="pln"> </span><span class="com"># 最小配置示例</span></span></p></li><li class="L1" data-node-id="20230412141928-ci2mn6u"><p><span role="presentation"><span class="pln">        url</span><span class="pun">:</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> https</span><span class="pun">:</span><span class="com">//www.twitch.tv/kimdaeyeob3</span></span></p></li><li class="L3" data-node-id="20230412141928-otqr8hg"><p><span role="presentation"><span class="pln">    </span><span class="pun">星际</span><span class="lit">2INnoVation</span><span class="pun">吕布卫星人族天梯第一视角:</span><span class="pln"> </span><span class="com"># 完整可选配置示例</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">        url</span><span class="pun">:</span></span></p></li><li class="L5" data-node-id="20230412141928-cuzgc35"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> https</span><span class="pun">:</span><span class="com">//www.twitch.tv/innovation_s2</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> https</span><span class="pun">:</span><span class="com">//www.panda.tv/1160340</span></span></p></li><li class="L7" data-node-id="20230412141928-4ur95bv"><p><span role="presentation"><span class="pln">        title</span><span class="pun">:</span><span class="pln"> </span><span class="str">"星际2INnoVation吕布卫星人族天梯第一视角%Y-%m-%d"</span><span class="pln"> </span><span class="com"># 自定义标题的时间格式</span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">        tid</span><span class="pun">:</span><span class="pln"> </span><span class="lit">171</span><span class="pln"> </span><span class="com"># 投稿分区码,174为生活，其他分区</span></span></p></li><li class="L9" data-node-id="20230412141928-yurvbxn"><p><span role="presentation"><span class="pln">        copyright</span><span class="pun">:</span><span class="pln"> </span><span class="lit">2</span><span class="pln"> </span><span class="com"># 1为自制</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">        cover_path</span><span class="pun">:</span><span class="pln"> </span><span class="str">/cover/</span><span class="pln">up</span><span class="pun">.</span><span class="pln">jpg </span><span class="com">#设置视频封面</span></span></p></li><li class="L1" data-node-id="20230412141928-a6q8sre"><p><span role="presentation"><span class="pln">        description</span><span class="pun">:</span><span class="pln"> </span><span class="pun">视频简介</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">        postprocessor</span><span class="pun">:</span><span class="pln"> </span><span class="com"># 上传完成后，将按自定义顺序执行自定义操作</span></span></p></li><li class="L3" data-node-id="20230412141928-h26gtap"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> run</span><span class="pun">:</span><span class="pln"> echo hello</span><span class="pun">!</span><span class="pln"> </span><span class="com"># 执行任意命令，等同于在shell中运行,视频文件路径作为标准输入传入</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> mv</span><span class="pun">:</span><span class="pln"> backup</span><span class="pun">/</span><span class="pln"> </span><span class="com"># 移动文件到backup目录下</span></span></p></li><li class="L5" data-node-id="20230412141928-8kq3dj5"><p><span role="presentation"><span class="pln">            </span><span class="com">#- rm # 删除文件，为默认操作</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pln">        tags</span><span class="pun">:</span></span></p></li><li class="L7" data-node-id="20230412141928-l0hjuos"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> </span><span class="pun">星际争霸</span><span class="lit">2</span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> </span><span class="pun">电子竞技</span></span></p></li><li class="L9" data-node-id="20230412141928-c3oqzyl"><p><span role="presentation"><span class="pln">        format</span><span class="pun">:</span><span class="pln"> mp4 </span><span class="com"># 视频保存格式</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">        opt_args</span><span class="pun">:</span><span class="pln"> </span><span class="com"># ffmpeg参数</span></span></p></li><li class="L1" data-node-id="20230412141928-x4jlk9w"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> </span><span class="str">'-ss'</span><span class="pln"> </span><span class="com"># 跳过开始的16秒</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">            </span><span class="pun">-</span><span class="pln"> </span><span class="str">'00:00:16'</span></span></p></li><li class="L3" data-node-id="20230412141928-betladx"><p><span class="pln"> </span></p></li><li class="L4"><p><span role="presentation"><span class="pun"></span></span></p></li></ol></pre>

**如果需要获取cookie，推荐Chrome商店搜索EditThisCookie插件来获取。**

**常用命令：**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="com">#biliup常用命令</span></span></p></li><li class="L1" data-node-id="20230412141928-p0tte6z"><p><span role="presentation"><span class="com"># 启动</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">python3 </span><span class="pun">-</span><span class="pln">m biliup start</span></span></p></li><li class="L3" data-node-id="20230412141928-pe5k2wv"><p><span role="presentation"><span class="com"># 退出 </span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">python3 </span><span class="pun">-</span><span class="pln">m biliup stop</span></span></p></li><li class="L5" data-node-id="20230412141928-j5j3ncv"><p><span role="presentation"><span class="com"># 重启 </span></span></p></li><li class="L6"><p><span role="presentation"><span class="pln">python3 </span><span class="pun">-</span><span class="pln">m biliup restart</span></span></p></li><li class="L7" data-node-id="20230412141928-s2iwkta"><p><span role="presentation"><span class="com"># 查看版本 </span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">python3 </span><span class="pun">-</span><span class="pln">m biliup </span><span class="pun">--</span><span class="pln">version</span></span></p></li><li class="L9" data-node-id="20230412141928-ws6t625"><p><span role="presentation"><span class="com"># 显示帮助以查看更多选项</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">python3 </span><span class="pun">-</span><span class="pln">m biliup </span><span class="pun">-</span><span class="pln">h</span></span></p></li></ol></pre>

**然后我们启动它就会全自动托管了。**

**这个非常适合作为接单工具。毕竟商业化不讲究那些乱七八糟的，只要能投入产出比高就行。只要一开始设置好后面就基本上不用管了。**

##### 2.2.3 API调用

**我们可以只把biliup当做一个库来调用。**

**上传**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="kwd">from</span><span class="pln"> biliup</span><span class="pun">.</span><span class="pln">plugins</span><span class="pun">.</span><span class="pln">bili_webup </span><span class="kwd">import</span><span class="pln"> </span><span class="typ">BiliBili</span><span class="pun">,</span><span class="pln"> </span><span class="typ">Data</span></span></p></li><li class="L1" data-node-id="20230412141928-w6om860"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">video </span><span class="pun">=</span><span class="pln"> </span><span class="typ">Data</span><span class="pun">()</span></span></p></li><li class="L3" data-node-id="20230412141928-nev4iz4"><p><span role="presentation"><span class="pln">video</span><span class="pun">.</span><span class="pln">title </span><span class="pun">=</span><span class="pln"> </span><span class="str">'视频标题'</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">video</span><span class="pun">.</span><span class="pln">desc </span><span class="pun">=</span><span class="pln"> </span><span class="str">'视频简介'</span></span></p></li><li class="L5" data-node-id="20230412141928-i2ofn8f"><p><span role="presentation"><span class="pln">video</span><span class="pun">.</span><span class="pln">source </span><span class="pun">=</span><span class="pln"> </span><span class="str">'添加转载地址说明'</span></span></p></li><li class="L6"><p><span role="presentation"><span class="com"># 设置视频分区,默认为160 生活分区</span></span></p></li><li class="L7" data-node-id="20230412141928-9hm1ncm"><p><span role="presentation"><span class="pln">video</span><span class="pun">.</span><span class="pln">tid </span><span class="pun">=</span><span class="pln"> </span><span class="lit">171</span></span></p></li><li class="L8"><p><span role="presentation"><span class="pln">video</span><span class="pun">.</span><span class="pln">set_tag</span><span class="pun">([</span><span class="str">'星际争霸2'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'电子竞技'</span><span class="pun">])</span></span></p></li><li class="L9" data-node-id="20230412141928-qtow7zr"><p><span role="presentation"><span class="kwd">with</span><span class="pln"> </span><span class="typ">BiliBili</span><span class="pun">(</span><span class="pln">video</span><span class="pun">)</span><span class="pln"> </span><span class="kwd">as</span><span class="pln"> bili</span><span class="pun">:</span></span></p></li><li class="L0"><p><span role="presentation"><span class="pln">    bili</span><span class="pun">.</span><span class="pln">login_by_password</span><span class="pun">(</span><span class="str">"username"</span><span class="pun">,</span><span class="pln"> </span><span class="str">"password"</span><span class="pun">)</span></span></p></li><li class="L1" data-node-id="20230412141928-9sco5qz"><p><span role="presentation"><span class="pln">    </span><span class="kwd">for</span><span class="pln"> file </span><span class="kwd">in</span><span class="pln"> file_list</span><span class="pun">:</span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">        video_part </span><span class="pun">=</span><span class="pln"> bili</span><span class="pun">.</span><span class="pln">upload_file</span><span class="pun">(</span><span class="pln">file</span><span class="pun">)</span><span class="pln">  </span><span class="com"># 上传视频</span></span></p></li><li class="L3" data-node-id="20230412141928-4arp7w7"><p><span role="presentation"><span class="pln">        video</span><span class="pun">.</span><span class="pln">append</span><span class="pun">(</span><span class="pln">video_part</span><span class="pun">)</span><span class="pln">  </span><span class="com"># 添加已经上传的视频</span></span></p></li><li class="L4"><p><span role="presentation"><span class="pln">    video</span><span class="pun">.</span><span class="pln">cover </span><span class="pun">=</span><span class="pln"> bili</span><span class="pun">.</span><span class="pln">cover_up</span><span class="pun">(</span><span class="str">'/cover_path'</span><span class="pun">).</span><span class="pln">replace</span><span class="pun">(</span><span class="str">'http:'</span><span class="pun">,</span><span class="pln"> </span><span class="str">''</span><span class="pun">)</span></span></p></li><li class="L5" data-node-id="20230412141928-fpuw5qd"><p><span role="presentation"><span class="pln">    ret </span><span class="pun">=</span><span class="pln"> bili</span><span class="pun">.</span><span class="pln">submit</span><span class="pun">()</span><span class="pln">  </span><span class="com"># 提交视频</span></span></p></li><li class="L6"><p><span role="presentation"><span class="pun"></span></span></p></li></ol></pre>

**下载**

<pre class="prettyprint linenums prettyprinted" spellcheck="false" lang=""><ol class="linenums"><li class="L0"><p><span role="presentation"><span class="kwd">from</span><span class="pln"> biliup</span><span class="pun">.</span><span class="pln">downloader </span><span class="kwd">import</span><span class="pln"> download</span></span></p></li><li class="L1" data-node-id="20230412141928-naqimjc"><p><span role="presentation"><span class="pun"></span></span></p></li><li class="L2"><p><span role="presentation"><span class="pln">download</span><span class="pun">(</span><span class="str">'文件名'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'https://www.panda.tv/1150595'</span><span class="pun">,</span><span class="pln"> suffix</span><span class="pun">=</span><span class="str">'flv'</span><span class="pun">)</span></span></p></li></ol></pre>

### 3、注意事项

**1、项目运行初期建议大家多去腾讯云的控制台多看看服务器资源占用情况。避免服务出现中断。也方便我们根据服务器状态来开拓业务。**

**2、上传投稿建议用小号，不推荐使用自己的大号。**

**3、注意保护你的服务器及B站账号安全，避免被入侵泄露。**

## 总结

**直播录像虽然看的人不多，但是基本上有一定数量的忠实粉丝的主播都不可或缺。同时也是在记录着主播的成长。如果是转载其他平台的视频、录播到B站获取收益也是有一定的受众的。**

**工具其实大家都会玩，怎么用这些工具获取更多的收益（无论是金钱还是流量还是粉丝）其实都是看个人怎么运作。**

**之前B站出现的卧龙寺、大霓奈靠工具搬运海量搞笑视频的账号个个都是几十万粉，本质上也都是一种应用罢了。甚至是把B站当做备份的索尼音乐（17.5万个投稿）也是。**

**不过我还是希望大家不要滥用，合理的规划，认真的对待自己的账号才是最好的。**

**参考：**

[https://github.com/biliup/biliup](https://github.com/biliup/biliup)

[https://github.com/BililiveRecorder/BililiveRecorder](https://github.com/BililiveRecorder/BililiveRecorder)

[https://blog.waitsaber.org/archives/129](https://blog.waitsaber.org/archives/129)
