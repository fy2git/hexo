---
abbrlink: ''
categories:
- - 教程
date: '2023-04-22 15:06:23'
tags:
- git
title: git项目
updated: Thu, 27 Apr 2023 06:48:23 GMT
---
* https://github.com/ClassmateLin/free-chatgpt

# free chatgpt

无需账号即可免费无限次数使用的chatgpt api, 通过[Vercel AI Playground](https://play.vercel.ai/)实现。

* 优点: 免费, 无需账号, 支持多个模型。
* 缺点: 不是很快, 不太稳定。

# demo

* url: `https://api.classmatelin.top/api`
* curl体验:

```
curl --silent --location --request POST 'https://api.classmatelin.top/api' \
--header 'Content-Type: application/json' \
--data-raw '{
 
        "model": "openai:gpt-3.5-turbo",
        "prompt": "您是一个Rust语言专家,我有问题需要问你。\n\n请问如何写一个hello world程序?"
}'
```

* [![usage](https://github.com/ClassmateLin/free-chatgpt/raw/main/images/usage.png)](https://github.com/ClassmateLin/free-chatgpt/blob/main/images/usage.png)
* vercel单个api仅支持256个token,通过多次请求合并上下文支持超过1024个token。 [![usage2](https://github.com/ClassmateLin/free-chatgpt/raw/main/images/usage2.png)](https://github.com/ClassmateLin/free-chatgpt/blob/main/images/usage2.png)

## 安装

* `docker run -itd -p 8080:8080 --name=free-chatgpt classmatelin/free-chatgpt:latest`.
* docker-compose, 见[lazy-docker](https://github.com/ClassmateLin/lazy-docker)

如需使用proxy, 则添加环境变量:

```
SOCKS_PROXY: "socks5:x.x.x.x:x"
HTTP_PROXY: "https://x.x.x.x:x"
HTTPS_PROXY: "https://x.x.x.x:x"
```

例如: `docker run -itd -p 8080:8080 -e SOCKS_PROXY="socks5:192.168.123.88:1080" --name=free-chatgpt classmatelin/free-chatgpt:latest`.

## 使用

```
curl --silent --location --request POST 'http://127.0.0.1:8080/api' \
--header 'Content-Type: application/json' \
--data-raw '{
 
        "model": "openai:gpt-3.5-turbo",
        "prompt": "您是一个Rust语言专家,我有问题需要问你。\n\n请问如何写一个hello world程序?"
}'
```

### 参数说明


| 参数              | 必填 | 描述                        |
| ----------------- | ---- | --------------------------- |
| model             | N    | 默认: openai:gpt-3.5-turbo. |
| temperature       | N    | 默认:1                      |
| topP              | N    | 默认:1                      |
| frequencyPenalty  | N    | 默认：0                     |
| presence\_penalty | N    | 默认:0                      |
| stop\_sequences   | N    | 默认:[]                     |

### 支持模型

* anthropic:claude-instant-v1
* anthropic:claude-v1
* replicate:replicate/alpaca-7b
* replicate:stability-ai/stablelm-tuned-alpha-7b
* huggingface:bigscience/bloomz
* huggingface:google/flan-t5-xxl
* huggingface:google/flan-ul2
* cohere:command-medium-nightly
* cohere:command-xlarge-nightly
* openai:gpt-3.5-turbo
* openai:text-ada-001
* openai:text-babbage-001
* openai:text-curie-001
* openai:text-davinci-002
* openai:text-davinci-003

**默认使用openai:gpt-3.5-turbo**

* https://github.com/pengzhile/pandora

## 如何运行

* Python版本目测起码要`3.7`
* pip安装运行

  ```shell
  pip install pandora-chatgpt
  pandora
  ```

  * 如果你想支持`gpt-3.5-turbo`模式：
    ```shell
    pip install 'pandora-chatgpt[api]'
    pandora
    ```
  * 如果你想启用`cloud`模式：
    ```shell
    pip install 'pandora-chatgpt[cloud]'
    pandora-cloud
    ```
* 编译运行

  ```shell
  pip install .
  pandora
  ```

  * 如果你想支持`gpt-3.5-turbo`模式：
    ```shell
    pip install '.[api]'
    pandora
    ```
  * 如果你想启用`cloud`模式：
    ```shell
    pip install '.[cloud]'
    pandora-cloud
    ```
* Docker Hub运行

  ```shell
  docker pull pengzhile/pandora
  docker run -it --rm pengzhile/pandora
  ```
* Docker编译运行

  ```shell
  docker build -t pandora .
  docker run -it --rm pandora
  ```
* 输入用户名密码登录即可，登录密码理论上不显示出来，莫慌。
* 简单而粗暴，不失优雅。

## 

## 程序参数

* 可通过 `pandora --help` 查看。
* `-p` 或 `--proxy` 指定代理，格式：`protocol://user:pass@ip:port`。
* `-t` 或 `--token_file` 指定一个存放`Access Token`的文件，使用`Access Token`登录。
* `-s` 或 `--server` 以`http`服务方式启动，格式：`ip:port`。
* `-a` 或 `--api` 使用`gpt-3.5-turbo`API请求，**你可能需要向`OpenAI`支付费用**。
* `--tokens_file` 指定一个存放多`Access Token`的文件，内容为`{"key": "token"}`的形式。
* `--sentry` 启用`sentry`框架来发送错误报告供作者查错，敏感信息**不会被发送**。
* `-v` 或 `--verbose` 显示调试信息，且出错时打印异常堆栈信息，供查错使用。

## 

## Docker环境变量

* `PANDORA_ACCESS_TOKEN` 指定`Access Token`字符串。
* `PANDORA_TOKENS_FILE` 指定一个存放多`Access Token`的文件路径。
* `PANDORA_PROXY` 指定代理，格式：`protocol://user:pass@ip:port`。
* `PANDORA_SERVER` 以`http`服务方式启动，格式：`ip:port`。
* `PANDORA_API` 使用`gpt-3.5-turbo`API请求，**你可能需要向`OpenAI`支付费用**。
* `PANDORA_SENTRY` 启用`sentry`框架来发送错误报告供作者查错，敏感信息**不会被发送**。
* `PANDORA_VERBOSE` 显示调试信息，且出错时打印异常堆栈信息，供查错使用。
* 使用Docker方式，设置环境变量即可，无视上述`程序参数`。

## 

## 关于 Access Token

* 使用`Access Token`方式登录，可以无代理直连。
* [这个服务](https://ai.fakeopen.com/auth) 可以帮你安全有效拿到`Access Token`，无论是否第三方登录。
* 其中`accessToken`字段的那一长串内容即是`Access Token`。
* `Access Token`可以复制保存，其有效期目前为`1个月`。
* 不要泄露你的`Access Token`，使用它可以操纵你的账号。

## 

## HTTP服务文档

* 如果你以`http`服务方式启动，现在你可以打开一个极简版的`ChatGPT`了。通过你指定的`http://ip:port`来访问。
* 通过`http://ip:port/?token=xxx`，传递一个Token的名字，可以切换到对应的`Access Token`。
* API文档见：[doc/HTTP-API.md](https://github.com/pengzhile/pandora/blob/master/doc/HTTP-API.md)

## 

## 操作命令

* 对话界面**连敲两次**`Enter`发送你的输入给`ChatGPT`。
* 对话界面使用`/?`可以打印支持的操作命令。
* `/title` 重新设置当前对话的标题。
* `/select` 回到选择会话界面。
* `/reload` 重新加载当前会话所有内容，`F5`你能懂吧。
* `/regen` 如果对`ChatGPT`当前回答不满意，可以让它重新回答。
* `/continue` 让`ChatGPT`继续输出回复的剩余部分。
* `/edit` 编辑你之前的一个提问。
* `/new` 直接开启一个新会话。
* `/del` 删除当前会话，回到会话选择界面。
* `/token` 打印当前的`Access Token`，也许你用得上，但不要泄露。
* `/copy` 复制`ChatGPT`上一次回复的内容到剪贴板。
* `/copy_code` 复制`ChatGPT`上一次回复的代码到剪贴板
* `/clear` 清屏，应该不用解释。
* `/version` 打印`Pandora`的版本信息。
* `/exit` 退出`潘多拉`。

## 

## 高阶设置

* 本部分内容不理解的朋友，**请勿擅动！**
* 环境变量 `OPENAI_API_PREFIX` 可以替换OpenAI Api的前缀`https://api.openai.com`。
* 环境变量 `CHATGPT_API_PREFIX` 可以替换ChatGPT Api的前缀`https://ai.fakeopen.com`。
* 如果你想持久存储`Docker`中`Pandora`产生的数据，你可以挂载宿主机目录至`/data`。
* 如果你在国内使用`pip`安装缓慢，可以考虑切换至腾讯的源：`pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple`
* 镜像同步版本可能不及时，如果出现这种情况建议切换至官方源：`pip config set global.index-url https://pypi.org/simple`
* 默认使用`sqlite3`存储会话数据，如果你希望更换至`mysql`，可以这么做：
  * 执行`pip install PyMySQL`安装驱动。
  * 设置环境变量：`DATABASE_URI`为类似`mysql+pymysql://user:pass@localhost/dbname`的连接字符串。
* 环境变量指定`OPENAI_EMAIL`可以替代登录输入用户名，`OPENAI_PASSWORD`则可以替代输入密码。
* 环境变量`API_SYSTEM_PROMPT`可以替换`api`模式下的系统`prompt`。

## 

## Cloud模式

* 搭建一个跟官方很像的`ChatGPT`服务，不能说很像，只能说一样。
* 该模式使用`pandora-cloud`启动，前提是你如前面所说安装好了。
* Docker环境变量：`PANDORA_CLOUD` 启动`cloud`模式。
* 该模式参数含义与普通模式相同，可`--help`查看。

## 

## 使用Cloudflare Workers代理

* 如果你感觉默认的`https://ai.fakeopen.com`在你那里可能被墙了，可以使用如下方法自行代理。
* 你需要一个`Cloudflare`账号，如果没有，可以[注册](https://dash.cloudflare.com/sign-up)一个。
* 登录后，点击`Workers`，然后点击`Create a Worker`，填入服务名称后点击`创建服务`。
* 点开你刚才创建的服务，点击`快速编辑`按钮，贴入下面的代码，然后点击`保存并部署`。

  ```js
  export default {
    async fetch(request, env) {
      const url = new URL(request.url);
      url.host = 'ai.fakeopen.com';
      return fetch(new Request(url, request))
    }
  }
  ```
* 点击`触发器`选项卡，可以添加自定义访问域名。
* 参考`高阶设置`中的环境变量使用你的服务地址进行替换。
* https://github.com/linweiyuan/go-chatgpt-api

# go-chatgpt-api

Bypass Cloudflare using [undetected\_chromedriver](https://github.com/ultrafunkamsterdam/undetected-chromedriver) to use ChatGPT API.

* [一种取巧的方式绕过 Cloudflare v2 验证](https://linweiyuan.github.io/2023/03/14/%E4%B8%80%E7%A7%8D%E5%8F%96%E5%B7%A7%E7%9A%84%E6%96%B9%E5%BC%8F%E7%BB%95%E8%BF%87-Cloudflare-v2-%E9%AA%8C%E8%AF%81.html)
* [ChatGPT 如何自建代理](https://linweiyuan.github.io/2023/04/08/ChatGPT-%E5%A6%82%E4%BD%95%E8%87%AA%E5%BB%BA%E4%BB%A3%E7%90%86.html)
* [一种解决 ChatGPT Access denied 的方法](https://linweiyuan.github.io/2023/04/15/%E4%B8%80%E7%A7%8D%E8%A7%A3%E5%86%B3-ChatGPT-Access-denied-%E7%9A%84%E6%96%B9%E6%B3%95.html)

---

Also support official API (the way which using API key):

* Chat completion

---

Use both ChatGPT mode and API mode:

```yaml
services:
  go-chatgpt-api:
    container_name: go-chatgpt-api
    image: linweiyuan/go-chatgpt-api
    ports:
      - 8080:8080
    environment:
      - GIN_MODE=release
      - CHATGPT_PROXY_SERVER=http://chatgpt-proxy-server:9515
#      - NETWORK_PROXY_SERVER=http://host:port
#      - NETWORK_PROXY_SERVER=socks5://host:port
    depends_on:
      - chatgpt-proxy-server
    restart: unless-stopped

  chatgpt-proxy-server:
    container_name: chatgpt-proxy-server
    image: linweiyuan/chatgpt-proxy-server
    restart: unless-stopped
```

---

Use API mode only:

```yaml
services:
  go-chatgpt-api:
    container_name: go-chatgpt-api
    image: linweiyuan/go-chatgpt-api
    ports:
      - 8080:8080
    environment:
      - GIN_MODE=release
#      - NETWORK_PROXY_SERVER=http://host:port
#      - NETWORK_PROXY_SERVER=socks5://host:port
    restart: unless-stopped
```

---

If your IP is blocked, like "Access denied", try this (with [Cloudflare WARP](https://developers.cloudflare.com/warp-client/get-started/linux)):

```yaml
services:
  go-chatgpt-api:
    container_name: go-chatgpt-api
    image: linweiyuan/go-chatgpt-api
    ports:
      - 8080:8080
    environment:
      - GIN_MODE=release
      - CHATGPT_PROXY_SERVER=http://chatgpt-proxy-server:9515
      - NETWORK_PROXY_SERVER=socks5://chatgpt-proxy-server-warp:65535
    depends_on:
      - chatgpt-proxy-server
      - chatgpt-proxy-server-warp
    restart: unless-stopped

  chatgpt-proxy-server:
    container_name: chatgpt-proxy-server
    image: linweiyuan/chatgpt-proxy-server
    environment:
      - LOG_LEVEL=INFO
    restart: unless-stopped

  chatgpt-proxy-server-warp:
    container_name: chatgpt-proxy-server-warp
    image: linweiyuan/chatgpt-proxy-server-warp
    environment:
      - LOG_LEVEL=INFO
    restart: unless-stopped
```

---

* https://github.com/its-star-boi/ngrok-rdp

## Description

**What is RDP?**

* RDP (Remote Desktop Protocol) is a network communications protocol  developed by Microsoft, which allows users to connect to another  computer from a remote location.

**How long does this RDP stay active?**

* This RDP stays active for up to 6 hours.

## 

## How to use it?

#### 

#### HOW TO USE:

[![](https://camo.githubusercontent.com/71ae14a5a69ce27294d86c77f968d8bad5dceae51139924708529a9a866c7589/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f486f77253230746f2532305573652d626c756576696f6c65743f7374796c653d666f722d7468652d6261646765266c6f676f3d6170707665796f72)](https://youtu.be/ZTSch-kF1xY)

#### 

#### First Step

1. Press the **fork** button
2. Login or signup to ngrok: [https://ngrok.com](https://ngrok.com)
3. Now visit here for token: [https://dashboard.ngrok.com/auth/your-authtoken](https://dashboard.ngrok.com/auth/your-authtoken)

> You'll get token from here. It'll be needed to the next step.

#### 

#### Second Step

1. In your forked repo: **Go to Settings > Secrets > Action > New Repository Secret**
2. In the name section, enter this text: **NGROK\_AUTH\_TOKEN**
3. In the value section, enter the **ngrok token**
4. Then press **Add Secret**
5. Now go to **Action > AWS (Left Menu) > Run Workflow**
6. Refresh the page and go to **AWS > build** option
7. You'll get IP, Username & Password from **Connect to RDP** section.

#### 

#### Third Step

1. Search **Remote Desktop Connection** from Windows Start Menu and open.
2. Put IP without **tcp://** and enter Username & click **Connect**.
3. Later on, put the password for credential/auth.

[![ss](https://camo.githubusercontent.com/2ac3c0f7bc72a759b393df482f5996eedd185ac6cd269c71c7c13fc685e739e4/68747470733a2f2f692e696d6775722e636f6d2f575172394e31412e706e67)](https://camo.githubusercontent.com/2ac3c0f7bc72a759b393df482f5996eedd185ac6cd269c71c7c13fc685e739e4/68747470733a2f2f692e696d6775722e636f6d2f575172394e31412e706e67)

## 

## Screenshots

[![ss](https://camo.githubusercontent.com/1ca60d0059d7d06d59bcfc63845076b6c031ff1938b7bf8611c7cc19a13bae76/68747470733a2f2f692e696d6775722e636f6d2f766744326f776b2e706e67)](https://camo.githubusercontent.com/1ca60d0059d7d06d59bcfc63845076b6c031ff1938b7bf8611c7cc19a13bae76/68747470733a2f2f692e696d6775722e636f6d2f766744326f776b2e706e67)

[![ss](https://camo.githubusercontent.com/ed219f3edfa7a54bbf38ca33e58e8b7207b824ee56f7825ac7279381a4bfabe7/68747470733a2f2f692e696d6775722e636f6d2f3858424c5571662e706e67)](https://camo.githubusercontent.com/ed219f3edfa7a54bbf38ca33e58e8b7207b824ee56f7825ac7279381a4bfabe7/68747470733a2f2f692e696d6775722e636f6d2f3858424c5571662e706e67)

## 

## License

The content of this project itself is licensed under the [Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/), and the underlying source code used to format and display that content is licensed under the [MIT License](https://github.com/its-star-boi/ngrok-rdp/blob/main/LICENSE).


* https://github.com/fscarmen2/Argo-Nezha-Service-Container

# Argo-Nezha-Service-Container


## 准备需要用的变量

* 通过 Cloudflare Json 生成网轻松获取 Argo 隧道信息: [https://fscarmen.cloudflare.now.cc](https://fscarmen.cloudflare.now.cc/)

[![image](https://user-images.githubusercontent.com/92626977/231084930-02e3c2de-c52b-420d-b39c-9f135d040b3b.png)](https://user-images.githubusercontent.com/92626977/231084930-02e3c2de-c52b-420d-b39c-9f135d040b3b.png)

* 到 Cloudflare 官方，在相应的域名 `DNS` 记录里加上客户端上报数据(tcp)和 ssh（可选）的域名，打开橙色云启用 CDN

[![image](https://user-images.githubusercontent.com/92626977/231087110-85ddab87-076b-45c9-97d1-c8b051dcb5b0.png)](https://user-images.githubusercontent.com/92626977/231087110-85ddab87-076b-45c9-97d1-c8b051dcb5b0.png)

[![image](https://user-images.githubusercontent.com/92626977/231087714-e5a45eb9-bc47-4c38-8f5b-a4a9fb492d0d.png)](https://user-images.githubusercontent.com/92626977/231087714-e5a45eb9-bc47-4c38-8f5b-a4a9fb492d0d.png)

* 到 Cloudflare 官方，选择使用的域名，打开 `网络` 选项将 `gRPC` 开关打开

[![image](https://user-images.githubusercontent.com/92626977/233138703-faab8596-a64a-40bb-afe6-52711489fbcf.png)](https://user-images.githubusercontent.com/92626977/233138703-faab8596-a64a-40bb-afe6-52711489fbcf.png)

* 获取 github 认证授权: [https://github.com/settings/applications/new](https://github.com/settings/applications/new)

面板域名加上 `https://` 开头，回调地址再加上 `/oauth2/callback` 结尾

[![image](https://user-images.githubusercontent.com/92626977/231099071-b6676f2f-6c7b-4e2f-8411-c134143cab24.png)](https://user-images.githubusercontent.com/92626977/231099071-b6676f2f-6c7b-4e2f-8411-c134143cab24.png)

[![image](https://user-images.githubusercontent.com/92626977/231086319-1b625dc6-713b-4a62-80b1-cc5b2b7ef3ca.png)](https://user-images.githubusercontent.com/92626977/231086319-1b625dc6-713b-4a62-80b1-cc5b2b7ef3ca.png)

* 获取 github 的 PAT (Personal Access Token): [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)

[![image](https://user-images.githubusercontent.com/92626977/233346036-60819f98-c89a-4cef-b134-0d47c5cc333d.png)](https://user-images.githubusercontent.com/92626977/233346036-60819f98-c89a-4cef-b134-0d47c5cc333d.png)

[![image](https://user-images.githubusercontent.com/92626977/233346508-273c422e-05c3-4c91-9fae-438202364787.png)](https://user-images.githubusercontent.com/92626977/233346508-273c422e-05c3-4c91-9fae-438202364787.png)

* 创建 github 用于备份的私库: [https://github.com/new](https://github.com/new)

[![image](https://user-images.githubusercontent.com/92626977/233345537-c5b9dc27-35c4-407b-8809-b0ef68d9ad55.png)](https://user-images.githubusercontent.com/92626977/233345537-c5b9dc27-35c4-407b-8809-b0ef68d9ad55.png)

## PaaS 部署实例

镜像 `fscarmen/argo-nezha:latest` ， 支持 amd64 和 arm64 架构

用到的变量


| 变量名           | 是否必须 | 备注                                                                                         |
| ---------------- | -------- | -------------------------------------------------------------------------------------------- |
| GH\_USER         | 是       | github 的用户名，用于面板管理授权                                                            |
| GH\_CLIENTID     | 是       | 在 github 上申请                                                                             |
| GH\_CLIENTSECRET | 是       | 在 github 上申请                                                                             |
| GH\_REPO         | 否       | 在 github 上备份哪吒服务端数据库文件的库                                                     |
| GH\_EMAIL        | 否       | github 的邮箱，用于备份的 git 推送到远程库                                                   |
| GH\_PAT          | 否       | github 的 PAT                                                                                |
| ARGO\_JSON       | 是       | 从[https://fscarmen.cloudflare.now.cc](https://fscarmen.cloudflare.now.cc/) 获取的 Argo Json |
| DATA\_DOMAIN     | 是       | 客户端与服务端的通信 argo 域名                                                               |
| WEB\_DOMAIN      | 是       | 面板 argo 域名                                                                               |
| SSH\_DOMAIN      | 否       | ssh 用的 argo 域名                                                                           |
| SSH\_PASSWORD    | 否       | ssh 的密码，只有在设置 SSH\_JSON 后才生效，默认值 password                                   |

Koyeb

[![Deploy to Koyeb](https://camo.githubusercontent.com/dbd49fd11e4dea39effabf3572eb66edafb50d32aadb31c7458fe7e42ac93790/68747470733a2f2f7777772e6b6f7965622e636f6d2f7374617469632f696d616765732f6465706c6f792f627574746f6e2e737667)](https://app.koyeb.com/deploy?type=docker&name=nezha&ports=80;http;/&env%5BGH_USER%5D=&env%5BGH_CLIENTID%5D=&env%5BGH_CLIENTSECRET%5D=&env%5BGH_REPO%5D=&env%5BGH_EMAIL%5D=&env%5BGH_PAT%5D=&env%5BARGO_JSON%5D=&env%5BDATA_DOMAIN%5D=&env%5BWEB_DOMAIN%5D=&env%5BSSH_DOMAIN%5D=&env%5BSSH_PASSWORD%5D=&image=docker.io/fscarmen/argo-nezha)

[![image](https://user-images.githubusercontent.com/92626977/231088411-fbac3e6e-a8a6-4661-bcf8-7c777aa8ffeb.png)](https://user-images.githubusercontent.com/92626977/231088411-fbac3e6e-a8a6-4661-bcf8-7c777aa8ffeb.png)

[![image](https://user-images.githubusercontent.com/92626977/231088973-7134aefd-4c80-4559-8e40-17c3be11d27d.png)](https://user-images.githubusercontent.com/92626977/231088973-7134aefd-4c80-4559-8e40-17c3be11d27d.png)

[![image](https://user-images.githubusercontent.com/92626977/233336491-6bb801af-257d-467d-aaf0-6dcb68a531ac.png)](https://user-images.githubusercontent.com/92626977/233336491-6bb801af-257d-467d-aaf0-6dcb68a531ac.png)

[![image](https://user-images.githubusercontent.com/92626977/231092893-c8f017a2-ee0e-4e28-bee3-7343158f0fa7.png)](https://user-images.githubusercontent.com/92626977/231092893-c8f017a2-ee0e-4e28-bee3-7343158f0fa7.png)

[![image](https://user-images.githubusercontent.com/92626977/231094144-df6715bc-c611-47ce-a529-03c43f38102e.png)](https://user-images.githubusercontent.com/92626977/231094144-df6715bc-c611-47ce-a529-03c43f38102e.png)

## VPS 部署实例

* 注意: ARGO\_JSON= 后面需要有单引号，不能去掉
* 如果 VPS 是 IPv6 only 的，请先安装 WARP IPv4 或者双栈: [https://github.com/fscarmen/warp](https://github.com/fscarmen/warp)
* 备份目录为当前路径的 dashboard 文件夹

### docker 部署

```
docker run -dit \
           --name nezha_dashboard \
           --restart always \
           -e GH_USER=<填 github 用户名> \
           -e GH_EMAIL=<填 github 邮箱> \
           -e GH_PAT=<填获取的> \
           -e GH_REPO=<填自定义的> \
           -e GH_CLIENTID=<填获取的>  \
           -e GH_CLIENTSECRET=<填获取的> \
           -e ARGO_JSON='<填获取的>' \
           -e WEB_DOMAIN=<填自定义的> \
           -e DATA_DOMAIN=<填自定义的> \
           -e SSH_DOMAIN=<填自定义的> \
           -e SSH_PASSWORD=<填自定义的> \
           fscarmen/argo-nezha
```

### docker-compose 部署

```
version: '3.8'
services:
    argo-nezha:
        image: fscarmen/argo-nezha
        container_name: nezha_dashboard
        restart: always
        environment:
            - GH_USER=<填 github 用户名>
            - GH_EMAIL=<<填 github 邮箱>
            - GH_PAT=<填获取的>
            - GH_REPO=<填自定义的>
            - GH_CLIENTID=<填获取的>
            - GH_CLIENTSECRET=<填获取的>
            - ARGO_JSON='<填获取的>'
            - WEB_DOMAIN=<填自定义的>
            - DATA_DOMAIN=<填自定义的>
            - SSH_DOMAIN=<填自定义的>
            - SSH_PASSWORD=<填自定义的>
```

## 客户端接入

通过gRPC传输，无需额外配置。使用面板给到的安装方式，举例

```
curl -L https://raw.githubusercontent.com/naiba/nezha/master/script/install.sh -o nezha.sh && chmod +x nezha.sh && sudo ./nezha.sh install_agent data.seales.nom.za 443 eAxO9IF519fKFODlW0 --tls
```

## SSH 接入

* 以 macOS + WindTerm 为例，其他根据使用的 SSH 工具，结合官方官方说明文档: [https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/use\_cases/ssh/#2-connect-as-a-user](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/use_cases/ssh/#2-connect-as-a-user)
* 官方 cloudflared 下载: [https://github.com/cloudflare/cloudflared/releases](https://github.com/cloudflare/cloudflared/releases)
* 以下输入命令举例

```
<file path>/cloudflared access ssh --hostname ssh.seales.nom.za
```

[![image](https://user-images.githubusercontent.com/92626977/233349393-cec79e11-346e-4a57-8357-8d153d75ee40.png)](https://user-images.githubusercontent.com/92626977/233349393-cec79e11-346e-4a57-8357-8d153d75ee40.png)

[![image](https://user-images.githubusercontent.com/92626977/233350601-73de67f9-19ca-451f-b395-8721abbb3342.png)](https://user-images.githubusercontent.com/92626977/233350601-73de67f9-19ca-451f-b395-8721abbb3342.png)

[![image](https://user-images.githubusercontent.com/92626977/233350802-754624e0-8456-4353-8577-1f5385fb8723.png)](https://user-images.githubusercontent.com/92626977/233350802-754624e0-8456-4353-8577-1f5385fb8723.png)

## 自动还原备份

* 把需要还原的文件名改到 github 备份库里的 `README.md`，定时服务会每分钟检测更新，并把上次同步的文件名记录在本地 `/dbfile` 处以与在线的文件内容作比对

下图为以还原文件名为 `dashboard-2023-04-23-13:08:37.tar.gz` 作示例

[![image](https://user-images.githubusercontent.com/92626977/233822466-c24e94f6-ba8a-47c9-b77d-aa62a56cc929.png)](https://user-images.githubusercontent.com/92626977/233822466-c24e94f6-ba8a-47c9-b77d-aa62a56cc929.png)

## 手动还原备份

* ssh 进入容器后运行，github 备份库里的 tar.gz 文件名，格式: dashboard-2023-04-22-21:42:10.tar.gz

```
bash /dashboard/restore.sh <文件名>
```

[![image](https://user-images.githubusercontent.com/92626977/233792709-fb37b79c-c755-4db1-96ec-1039309ff932.png)](https://user-images.githubusercontent.com/92626977/233792709-fb37b79c-c755-4db1-96ec-1039309ff932.png)

## 完美搬家

* 备份原哪吒的 `/dashboard` 文件夹，压缩备份为 `dashboard.tar.gz` 文件

```
tar czvf dashboard.tar.gz /dashboard
```

* 下载文件并放入私库，这个私库名要与新哪吒 <GH\_REPO> 完全一致，并把该库的 README.md 的内容编辑为 `dashboard.tar.gz`
* 部署本项目新哪吒，完整填入变量即可。部署完成后，自动还原脚本会每分钟作检测，发现有新的内容即会自动还原，全程约 3 分钟

## 主体目录文件及说明

```
.
|-- dashboard
|   |-- app                  # 哪吒面板主程序
|   |-- argo.json            # Argo 隧道 json 文件，记录着使用隧道的信息
|   |-- argo.yml             # Argo 隧道 yml 文件，用于在一同隧道下，根据不同域名来分流 web, gRPC 和 ssh 协议的作用
|   |-- backup.sh            # 备份数据脚本
|   |-- data
|   |   |-- config.yaml      # 哪吒面板的配置，如 Github OAuth2 / gRPC 域名 / 端口 / 是否启用 TLS 等信息
|   |   `-- sqlite.db        # SQLite 数据库文件，记录着面板设置的所有 severs 和 cron 等信息
|   |-- entrypoint.sh        # 主脚本，容器运行后执行
|   |-- nezha-agent          # 哪吒客户端，用于监控本地 localhost
|   |-- nezha.csr            # SSL/TLS 证书签名请求
|   |-- nezha.key            # SSL/TLS 证书的私钥信息
|   |-- nezha.pem            # SSL/TLS 隐私增强邮件
|   `-- restore.sh           # 还原备份脚本
`-- dbfile                   # 记录最新的还原或备份文件名
```
