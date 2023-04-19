---
abbrlink: ''
categories: []
date: '2023-04-19T10:53:17.117901+08:00'
tags: []
title: 'Docker命令 '
updated: 2023-4-19T10:53:19.898+8:0
---
查看运行容器

docker ps

1
查看所有容器

docker ps -a

1
进入容器

其中字符串为容器ID:

docker exec -it d27bd3008ad9 /bin/bash

1
1.停用全部运行中的容器:

docker stop $(docker ps -q)

1
2.删除全部容器：

docker rm $(docker ps -aq)

1
3.一条命令实现停用并删除容器：

————————————————
版权声明：本文为CSDN博主「jeikerxiao」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/jeikerxiao/article/details/78476925
