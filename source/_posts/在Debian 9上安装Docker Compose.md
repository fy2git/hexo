---
abbrlink: ''
categories: []
date: '2023-04-20 17:44:55'
tags: []
title: 在Debian 9上安装Docker Compose
updated: Thu, 20 Apr 2023 09:44:56 GMT
---
## **介绍**

[Docker](https://docs.docker.com/)是一个很好的工具，用于在软件[容器](https://cloud.tencent.com/product/tke?from=20065&from_column=20065)中自动部署Linux应用程序，但要充分利用其潜力，应用程序的每个组件都应该在自己的单独容器中运行。对于具有大量组件的复杂应用程序，编排所有容器以启动，通信和关闭可能很快变得难以处理。

Docker社区提出了一个名为[Fig](http://www.fig.sh/)的流行解决方案，它允许您使用单个YAML文件来编排所有Docker容器和配置。这变得如此受欢迎，以至于Docker团队决定基于Fig源制作[Docker Compose](https://docs.docker.com/compose/)，现在已弃用。Docker Compose使用户可以更轻松地编排Docker容器的进程，包括启动，关闭和设置容器内链接和卷。

在本教程中，我们将向您展示如何安装最新版本的Docker Compose，以帮助您管理Debian 9[服务器](https://cloud.tencent.com/product/cvm?from=20065&from_column=20065)上的多容器应用程序。

## **先决条件**

要阅读本文，您需要：

* Debian 9服务器和具有sudo权限的非root用户。没有服务器的同学可以在[这里购买](https://cloud.tencent.com/product/cvm)，不过我个人更推荐您使用**免费**的腾讯云[开发者实验室](https://cloud.tencent.com/developer/labs)进行试验，学会安装后再[购买服务器](https://cloud.tencent.com/product/cvm)。
* [使用Debian 9教程的初始服务器设置](https://cloud.tencent.com/developer/article/1359642)解释了如何设置它。

**注意：**尽管前提条件提供了在Debian 9上安装Docker的说明，但只要安装了Docker，本文中的`docker`命令就可以在其他操作系统上运行。

## **第1步 - 安装Docker Compose**

虽然我们可以从官方Debian存储库安装Docker Compose，但它是最新版本背后的几个次要版本，所以我们将从Docker的GitHub存储库安装它。以下命令与您在“ [版本”](https://github.com/docker/compose/releases)页面上找到的命令略有不同。通过使用`-o`标志首先指定输出文件而不是重定向输出，此语法可避免遇到使用`sudo`时导致的权限被拒绝错误。

我们将检查[当前版本](https://github.com/docker/compose/releases)，如有必要，请在以下命令中更新它：

```javascript
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

接下来我们将设置权限：

```javascript
sudo chmod +x /usr/local/bin/docker-compose
```

然后我们将通过检查版本来验证安装是否成功：

```javascript
docker-compose --version
```

这将打印出我们安装的版本：

```javascript
Outputdocker-compose version 1.22.0, build f46880fe
```

现在我们已经安装了Docker Compose，我们已准备好运行“Hello World”示例。

## **第2步 - 使用Docker Compose运行容器**

公共Docker注册表Docker Hub包含一个用于演示和测试的*Hello World*图像。它说明了使用Docker Compose运行容器所需的最小配置：调用单个映像的YAML文件。我们将创建这个最小配置来运行我们的`hello-world`容器。

首先，我们将为YAML文件创建一个目录并移入其中：

```javascript
mkdir hello-world
cd hello-world
```

然后，我们将创建YAML文件：

```javascript
nano docker-compose.yml
```

将以下内容放入文件，保存文件，然后退出文本编辑器：

```javascript
my-test:
 image: hello-world
```

YAML文件中的第一行用作容器名称的一部分。第二行指定用于创建容器的图像。当我们运行`docker-compose up`命令时，它将按我们指定的`hello-world`名称查找本地图像。有了这个，我们将保存并退出该文件。

我们可以使用以下`docker images`命令手动查看系统上的图像：

```javascript
docker images
```

当根本没有本地图像时，只显示列标题：

```javascript
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

现在，当我们仍然在`~/hello-world`目录中时，我们将执行以下命令：

```javascript
docker-compose up
```

我们第一次运行命令时，如果没有名叫`hello-world`的本地映像，Docker Compose将从Docker Hub公共存储库中提取它：

```javascript
Pulling my-test (hello-world:)...
latest: Pulling from library/hello-world
9db2ca6ccae0: Pull complete
Digest: sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc
Status: Downloaded newer image for hello-world:latest
. . .
```

拉动图像后，`docker-compose`创建一个容器，附加并运行[hello](https://github.com/docker-library/hello-world/blob/85fd7ab65e079b08019032479a3f306964a28f4d/hello-world/Dockerfile)程序，然后确认安装似乎正在工作：

```javascript
. . .
Creating helloworld_my-test_1...
Attaching to helloworld_my-test_1
my-test_1 |
my-test_1 | Hello from Docker.
my-test_1 | This message shows that your installation appears to be working correctly.
my-test_1 |
. . .
```

然后它打印出它所做的解释：

```javascript
 To generate this message, Docker took the following steps:
my-test_1  |  1. The Docker client contacted the Docker daemon.
my-test_1  |  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
my-test_1  |     (amd64)
my-test_1  |  3. The Docker daemon created a new container from that image which runs the
my-test_1  |     executable that produces the output you are currently reading.
my-test_1  |  4. The Docker daemon streamed that output to the Docker client, which sent it
my-test_1  |     to your terminal.
```

Docker容器只在命令处于活动状态时才运行，因此一旦`hello`完成运行，容器就会停止。因此，当我们查看活动进程时，将显示列标题，但不会列出`hello-world`容器，因为它没有运行：

```javascript
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
```

我们可以通过使用`-a`标志来查看容器信息，我们将在下一步中使用它们。这显示了所有容器，而不仅仅是活动容器：

```javascript
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
06069fd5ca23        hello-world         "/hello"            35 minutes ago      Exited (0) 35 minutes ago                       hello-world_my-test_1
```

这将显示我们完成后删除容器所需的信息。

## **第3步 - 删除图像（可选）**

为避免使用不必要的磁盘空间，我们将删除本地映像。为此，我们需要使用`docker rm`命令删除引用该图像的所有容器，然后删除`CONTAINER ID`或者`NAME`。下面，我们正在使用来自我们刚刚运行的`docker ps -a`命令的`CONTAINER ID`。请务必替换容器的ID：

```javascript
docker rm 06069fd5ca23
```

一旦删除了引用该图像的所有容器，我们就可以删除该图像：

```javascript
docker rmi hello-world
```

## **结论**

我们现在已经安装了Docker Compose，通过运行Hello World示例测试了我们的安装，并删除了测试图像和容器。

虽然Hello World示例确认了我们的安装，但简单的配置并没有显示Docker Compose的主要优点之一 - 能够同时上下一组Docker容器。
