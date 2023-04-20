---
abbrlink: ''
categories:
- - 技巧
date: '2023-04-20 09:28:35'
tags:
- screen
title: screen的一些常用命令和快捷键
updated: Thu, 20 Apr 2023 10:21:18 GMT
---
1 安装screen

在linux上安装screen时候

apt-get install screen

报错：

E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
看来需要更高的sudo权限。

因此使用

sudo apt-get install screen

输入密码，然后就可以顺利安装了。

$ sudo apt-get install screen
[sudo] password for LIST_2080Ti:
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
dctrl-tools dkms libatomic1:i386 libbsd0:i386 libdrm-amdgpu1:i386 libdrm-intel1:i386 libdrm-nouveau2:i386 libdrm-radeon1:i386 libdrm2:i386 libedit2:i386 libelf1:i386
libexpat1:i386 libffi7:i386 libgl1:i386 libgl1-mesa-dri:i386 libglapi-mesa:i386 libglvnd0:i386 libglx-mesa0:i386 libglx0:i386 libllvm12:i386 libnvidia-cfg1-515-server
libnvidia-compute-515-server libnvidia-compute-515-server:i386 libnvidia-decode-515-server libnvidia-decode-515-server:i386 libnvidia-encode-515-server
libnvidia-encode-515-server:i386 libnvidia-extra-515-server libnvidia-fbc1-515-server libnvidia-fbc1-515-server:i386 libpciaccess0:i386 libsensors5:i386 libstdc++6:i386
libvulkan1:i386 libwayland-client0:i386 libx11-6:i386 libx11-xcb1:i386 libxau6:i386 libxcb-dri2-0:i386 libxcb-dri3-0:i386 libxcb-glx0:i386 libxcb-present0:i386
libxcb-randr0:i386 libxcb-shm0:i386 libxcb-sync1:i386 libxcb-xfixes0:i386 libxcb1:i386 libxdmcp6:i386 libxext6:i386 libxfixes3:i386 libxshmfence1:i386 libxxf86vm1:i386
mesa-vulkan-drivers:i386 nvidia-compute-utils-515-server nvidia-dkms-515-server nvidia-kernel-common-515-server nvidia-kernel-source-515-server nvidia-utils-515-server
xserver-xorg-video-nvidia-515-server
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
libutempter0
Suggested packages:
byobu | screenie | iselect
The following NEW packages will be installed:
libutempter0 screen
0 upgraded, 2 newly installed, 0 to remove and 16 not upgraded.
Need to get 586 kB of archives.
After this operation, 1,073 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal/main amd64 libutempter0 amd64 1.1.6-4 [8,256 B]
Get:2 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates/main amd64 screen amd64 4.8.0-1ubuntu0.1 [577 kB]
Fetched 586 kB in 0s (1,777 kB/s)
Selecting previously unselected package libutempter0:amd64.
(Reading database ... 197917 files and directories currently installed.)
Preparing to unpack .../libutempter0_1.1.6-4_amd64.deb ...
Unpacking libutempter0:amd64 (1.1.6-4) ...
Selecting previously unselected package screen.
Preparing to unpack .../screen_4.8.0-1ubuntu0.1_amd64.deb ...
Unpacking screen (4.8.0-1ubuntu0.1) ...
Setting up libutempter0:amd64 (1.1.6-4) ...
Setting up screen (4.8.0-1ubuntu0.1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for install-info (6.7.0.dfsg.2-5) ...
Processing triggers for libc-bin (2.31-0ubuntu9.9) ...
/sbin/ldconfig.real: /lib/i386-linux-gnu/libGL.so.1 is not a symbolic link

/sbin/ldconfig.real: /lib/x86_64-linux-gnu/libstdc++.so.6 is not a symbolic link

Processing triggers for systemd (245.4-4ubuntu3.19) ...
如上所示，screen就安装成功了。
2 创建-查询-进入-退出-删除

创建窗口并命名
screen -S start1
查询所有窗口名字
screen -ls
退出当前窗口，快捷键：Ctrl+A+D

重新进入窗口
screen -r start1
这就是我创建的两个窗口名字：

(base) LIST_2080Ti@ubuntu-SYS-7049GP-TRT:~/2080/CHB-MIT-DATA/epilepsy_eeg_classification$ screen -ls
There are screens on:
2076618.start1  (02/05/2023 11:07:16 PM)        (Attached)
2073921.train   (02/05/2023 10:53:28 PM)        (Attached)
2 Sockets in /run/screen/S-LIST_2080Ti.
(base) LIST_2080Ti@ubuntu-SYS-7049GP-TRT:~/2080/CHB-MIT-DATA/epilepsy_eeg_classification$
右上角显示你创立的窗口。

如果有事离开关闭电脑，可以用快捷键 Ctrl+a d(即按住 Ctrl，依次再按 a,d)，而会话中的程序不会关闭，仍在运行。

删除窗口

screen -X -S 2076618.start1 quit

screen -X -S 2073921.train quit
3 screen常用快捷键

快捷键
ctrl a ctrl a，最近使用的两个窗口之间切换
ctrl a + 数字，切换到某个窗口
ctrl a + d，detach
ctrl a + k，关闭当前窗口
ctrl a + :，进入命令行模式
ctrl a a，screen的快捷键的prefix默认是ctrl+a，这与bash中的快捷键（ctrl+a，回到命令开头）冲突，在screen要先按ctrl + a，再按a就可以了，注意不要和窗口切换的快捷键弄混
ctrl a + [，进入复制模式，这个我用来翻屏
ctrl a + A，修改当前窗口的名称

4 如何使用screen运行程序

步骤1：创建screen
screen -S screen_name
步骤2：激活虚拟环境
conda activate conda_name
步骤3：在你的项目文件夹下运行
python run.py
步骤4：退出服务器该运行界面
Ctrl+A+D
步骤5：恢复screen界面查看程序运行情况
screen -r screen_name
步骤6：执行完程序，关闭窗口
screen -X -S screen_name quit

如果不记得screen的名字，可以查询你所创建的所有窗口
screen -ls
