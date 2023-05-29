---
abbrlink: ''
categories:
- - 教程
date: '2023-05-29 20:56:18'
tags:
- B2
- CF
title: 搭配CF带宽联盟实现流量免费
updated: Mon, 29 May 2023 12:56:18 GMT
---
> CloudFlare 是全球知名的 CDN 服务提供商，其与诸多云服务商构建的带宽联盟为末端用户提供了极大的流量优惠。

【对象存储】搭配 CF 带宽联盟实现流量免费
========================================

CloudFlare 是全球知名的 CDN 服务提供商，其与诸多云服务商构建的带宽联盟为末端用户提供了极大的流量优惠。本文将结合博主在 Backblaze（B2）和阿里云 OSS 使用上的实践探索，简单展示在搭配使用 CloudFlare CDN 和保护源对象存储的一些技巧，希望大家能够喜欢~

顺便值此新春佳节之际，感谢诸位一直以来对博主的支持，简单地新的一年祝大家阖家欢乐、学习进步、事业有成哦啊♪(´▽｀)~

---

一、概述
--------

### 带宽联盟

带宽联盟是由 CloudFlare 主导组建的旨在节省云计算服务商间数据流量开销的组织，换句话说就是节省 IDC 间的流量传输，减少各 IDC 通过基础 ISP 向互联网传输资源支付的费用。在这个联盟中，受益最大的就是 CF 和末端用户，CF 借此巩固自身的互联网带宽权益地位，更有底气向 ISP 压低带宽价格，用户则获得了来自服务商的流量费用豁免。目前根据实际测试，带宽联盟的流量节省方式主要是 CF 与服务商构建对等互联或选择低价的 ISP/IXP 完成互联。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-02_09-31-09.png)

虽然加入带宽联盟的服务商有二十余家，不过是否向末端用户提供优惠仍取决于各服务商自身的政策。值得一提的是，像 AWS 和 Google 这样在互联权益地位很高的服务商，对带宽联盟基本是持拒绝或仅提供基础支持的态度，这是因为利益的冲突驱使他们不愿向 CF 转让自己的权益优势，从 IDC 的角度来看这是无可厚非的。

> #官方简介
> [https://www.cloudflare.com/zh-cn/bandwidth-alliance](https://www.cloudflare.com/zh-cn/bandwidth-alliance)

### 支持的服务商

正如上文所言，并不是所有参与带宽联盟的服务商都向用户提供这样的优惠，以下推荐是博主和朋友亲自测试确认的结果，欢迎大家进行补充。

**（1）阿里云**：阿里云 OSS 已确认除迪拜和中国大陆的地域外流量传出至 CF 边缘节点不计费。计费项目包括存储费用和请求费用，国内版产品定价（[点击前往](https://www.aliyun.com/price/product#/oss/detail/ossbag)），国际版产品定价（[点击前往](https://www.alibabacloud.com/zh/product/oss/pricing)）。注意，仅有国际版客户拥有每月 1 亿次的免费读请求额度。其优点是功能完善，地域众多且网络对大陆友好，有各类资源包供选购；缺点是费用偏高，计费方式复杂。

> #官方说明
> [https://www.aliyun.com/product/news/detail?id=17749](https://www.aliyun.com/product/news/detail?id=17749)

**（2）Backblaze（B2）**：Cloud Storage 已确认流量传出至 CF 边缘节点不计费。计费项目包括存储费用（0.005/G· 月）及列目录、读取的 B/C 类请求费用（0.004 / 万次），产品定价（[点击前往](https://www.backblaze.com/b2/cloud-storage-pricing.html)）。其优点是价格非常友好且计费灵活；缺点是地域不可选、功能很少、可用区网络质量较差。

> #官方说明
> [https://www.backblaze.com/b2/solutions/content-delivery.html](https://www.backblaze.com/b2/solutions/content-delivery.html)

**（3）DigitalOcean**：Spaces Object Storage 已确认流量传出至 CF 边缘节点不计费。计费项目只有存储费用，不计请求费用，但需要选择 5USD / 月的基础套餐，产品定价（[点击前往](https://www.digitalocean.com/products/spaces)）。博主并没有亲自使用这个服务因此不做过多评价，显而易见的优点是计费非常简单；缺点是不够灵活，仅有一种包计费模式。

> #官方说明
> [https://www.digitalocean.com/community/questions/bandwidth-alliance-status](https://www.digitalocean.com/community/questions/bandwidth-alliance-status)

---

二、连接 CloudFlare
-------------------

连接到 CloudFlare 需要为存储桶绑定域名，然后在 CloudFlare 设置 CNAME 到桶域名。像阿里云 OSS、腾讯云 COS 等经过自行开发的是可以直接通过相应的域名绑定页面进行绑定，其余的 AWS S3、Scaleway 之类的是通过设置 bucket 名称为域名进行绑定，只有 B2 选择的方式非常特别。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_01-19-03.png)

另外建议为自定义的域名在 CloudFlare 添加一条页面规则，将 SSL 设置为灵活（即 HTTP 回源），因为多数对象存储不支持自定义域名 SSL 访问，选用 HTTPS 回源的话是不必要的（但是 B2 是不允许 HTTP 访问的就必须要设置为 HTTPS 回源）。其余的页面规则可以按照自己按照其需求自行添加，在此不做赘述。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_01-29-22.jpg)

### 阿里云 OSS 等

阿里云绑定自定义域名很简单，在【传输管理】-【域名管理】中，点击绑定域名按流程即可完成绑定。需要在 CloudFlare 指向的桶域名，也可以在概览中的外网访问 Bucket 域名找到。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_01-22-07.jpg)

### Backblaze

B2 选择的绑定方式就非常特别了，在 bucket 文件管理页面上传一个文件，点击右侧信息按钮可以在其中获得`Friendly URL`，将你的域名 CNAME 指向链接中域名比如`f004.backblazeb2.com`后，就可以将链接中域名替换为自己的域名实现访问。B2 的服务器是向任意域名接入，bucket 间域名公用仅以目录区分，这样存在的隐患十分明显，文件链接完全公开而且自定义域名可被其他用户使用。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_01-53-16.jpg)

CloudFlare 近期的【转换规则】可以完美解决这个问题，在【创建转换规则】下选择【重写 URL】，传入匹配建议如图选择相应主机名，随后在路径下找到重写到，选择 Dynamic 动态模式，按如图修改`/file/lms-example`为即需要隐藏的目录填入框内即可。这样访问链接中 bucket 路径便被隐藏了，既美观也减少了被滥用的可能。

```
#隐藏目录中/file/lms-example部分
concat("/file/lms-example", http.request.uri.path)
```

同样的，利用转换规则隐藏部分目录也可以用于其他的对象存储服务商，因为绝大多数的对象存储公共访问链接是有迹可循的，被有心之人找出并利用可能会产生巨额的流量费用。我们可以在 bucket 中新建一个用于存放对外访问的文件夹，然后与 B2 的操作一样将该目录通过动态重写隐藏掉，这些可以自行发挥。

---

三、隐藏 Bucket 特征
--------------------

### 设置错误页

S3 协议的 bucket 在公开读的权限下会默认展示桶内文件列表和路径，特征明显而且并不友好。绝大多数的对象存储都支持设置静态网站`index.html`和错误页`404.html`的设置，比如阿里云 OSS 在【基础设置】-【静态页面】下。这里随手找了两个单页供选择和参考（[点击前往](https://static.lty.fun/?%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90/%E5%8D%95%E9%A1%B5/%E9%94%99%E8%AF%AF%E9%A1%B5)）。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_02-21-01.jpg)

显然，像 B2 这种简单粗暴的是又一次没有这样的功能，可以如图使用 URL 重写规则将主页静态定向至 index.html。404 页面的功能没有一个比较好的办法，不过 B2 的友好访问链接不会展示目录列表和 bucket 信息，影响并不大。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_08-11-44.jpg)

### 隐藏 bucket 标头

在对象存储的标头中会包含有一些 bucket 信息，可以通过控制台的 Network 选项卡暴露出来，也是对对象存储特征的一个暴露点。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_10-13-24.jpg)

通过 CloudFlare 转换规则中的【修改响应头】，能够简单地实现响应头的去除，同时还可以加入跨域请求头等需要的标头。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_10-08-11.jpg)

目前 B2 和 OSS 可以隐藏的标头大致如下，其他对象存储请根据实际情况去查看和配置，在此就不一一列出了。

<table><tbody><tr><td><strong>阿里云 OSS</strong></td><td><strong>Backblaze B2</strong></td></tr><tr><td>x-oss-hash-crc64ecma</td><td>x-bz-content-sha1</td></tr><tr><td>x-oss-object-type</td><td>x-bz-file-id</td></tr><tr><td>x-oss-request-id</td><td>x-bz-file-name</td></tr><tr><td>x-oss-server-time</td><td>x-bz-info-src_last_modified_millis</td></tr><tr><td>x-oss-storage-class</td><td>x-bz-upload-timestamp</td></tr></tbody></table>

### 禁止访问文件

同样的，通过页面规则中的 URL 重写功能，我们也能够实现诸如对特定目录、特定后缀（如图`.php`）的禁止访问，重写至错误页面即可。不过需要注意的是，转换规则是下方规则优先级更高，而页面规则是上方规则优先级更高。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_10-11-43.jpg)

---

四、CloudFlare 安全规则
-----------------------

### 页面规则

CloudFlare 页面规则前文有提到，免费版提供 3 条规则，在这里也可以设置一些边缘缓存有效期、缓存内容等等细致的配置。博主来自对象存储的静态资源一般进行如下的配置，如果有特殊的需要可以自行结合实际去探索。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_08-23-16.jpg)

### 防火墙规则

CloudFlare 防火墙有 5 条免费的防火墙规则，入口在【防火墙】-【防火墙规则】，能够实现很细致的安全规则，比如图中配置即为开启验证码。其他的项目在这里暂时不做过多的讲解，等后续有时间，我们再展开详细聊一聊 CloudFlare 防火墙的规则应用。

![](https://cdn.luotianyi.vc/wp-content/uploads/2022-02-03_08-20-43.jpg)

---

五、结语
--------

此外还有一些对象存储比如 Scaleway 提供每个月免费 75G 的空间和流量且不计请求，腾讯云 COS 的带宽联盟优惠政策也在推进
