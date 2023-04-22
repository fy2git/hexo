---
abbrlink: ''
categories: []
date: '2023-04-22 15:06:23'
tags: []
title: git项目
updated: Sat, 22 Apr 2023 07:06:24 GMT
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
