---
title: kali-Linux 用户组
date: 2021/9/15 23:29:50
tags:
  - Kali Linux
  - 教学文档
categories:
  - 程序
cover: https://tse3-mm.cn.bing.net/th/id/OIP-C.RI-jGvsfW8kRiYJdf5eqlwHaD4?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

## kali-Linux 用户组

`Lali Linux 系列第二篇`

### 用户组介绍

> 在 Linux 中，每个文件都是属于一个用户的，而每个用户都属于一个用户组
>
> 在 Linux 中，可以同时拥有N多个用户，和用户组
>
> 在 Linux 中，用户分为三类：系统用户（Root用户）、普通用户（指用户所创建的用户）、虚拟用户（指软件所创建的虚拟用户）

**用户信息解释**

> bai : x :1001 : 0 :: /home/bai:/bin/sh
>
> bai：表示用户名
>
> ":"：表示分割线
>
> x：表示密码，我们看到是x，其实密码已经映射到 /etc/shadow 文件中，也就是为了保密
>
> 1001：表示用户UID
>
> 0：表示用户组
>
> /home/bai：表示用户的目录
>
> ---
>
> ":"：为分隔符
>
> 第一个root为用户名
>
> 第二个x为密码
>
> 第三个0为UID
>
> 第四个0为GID
>
> 第五个root为用户全名
>
> 第六个/root为用户的家目录所在位置：beinan这个用户
>
> 第七/bin/bash为用户所用SHELL的类型

`还是不明白可以参考`：[说明文件](https://blog.51cto.com/983836259/1634705)

**用户组解释**

> root : x : 0 : root,key
>
> ":"为分隔符
>
> 第一个 root 为组名
>
> 第二个 x 为密码，保存在 shadow文件
>
> 第三个 0 为组ID
>
> 第四个root为组成员
>
> 第五个Key代表第二个组成员

`还是不明白可以参考`[说明文件](https://blog.csdn.net/fu18838928050/article/details/82454127)

**在 Linux 中查看用户信息使用下面的指令**

> cat /etc/passwd

**在 Linux 中查看用户组信息使用下面的指令**

> cat /etc/group

**查看指定的用户信息**

> id <用户名称>

**查看指定的用户组信息**

> id <组名>

### 用户组的概念

> 文件拥有者：就是文件的创作者，谁创建的文件，谁就是文件的所有者

> 用户所在组：当 Linux 当前用户创建了一个文件后，则这个文件所属于的组便是这个用户所在的组

> 其他组：除了文件所有者以及所在组的用户外，系统其他的用户都是文件的其他组

### 用户管理

```v
useradd --help  	--- 查看帮助命令
cat /etc/passwd		--- 查看用户信息
useradd -m <用户名>  --- 自动在home文件夹下创建<用户>目录 - 添加用户
useradd -g <组名> <用户名> --- 指定添加在某个组里
passwod <用户名>	  --- 修改用户的新密码
userdel -r <用户名>  --- 删除目录以及用户
groups <用户名>      --- 查看用户组归属
usermod -g <组名> <用户名> --- 添加组 直接添加组用用户从原来组
usermod -a-G <组名> <用户名> --- 追加到新的组
groupdel <组名> 删除组
```

### 权限管理

#### 权限划分

> r 表示读文件 数值为4
>
> w 表示写文件 数值为2
>
> x 表示执行 数值为1

#### 查看权限

> 当前目录下 ls -l

```v
d rwx r-x r-x. 2 root root 36864 Aug 9 19:37 bin
d rwx r-x r-x. 2 root root 4096  Jun 28 2011 eyc
d rwx r-x r-x. 2 root root 4096  Jun 28 2011 games
d rwx r-x r-x. 3 root root 4096  Aug 7 19:00 include
```

> d 表示文件类型：
>
> - "-" 普通文件
> - d   目录文件
> - p   管理文件
> - l    链接文件
> - b   块设备文件
> - c   字符设备文件
> - s   套接字文件   

> rwx r-x r-x. 表示文件权限：r 读权限 w 写权限 x 可执行权限 - 无权限
>
> rwx 表示所有者 / 所有者权限
>
> r-x 组用户权限（一个组中除所有者拥有的权限）
>
> r-x. 其他用户权限（除过当前有所有者的组，其他组可以进行访问的权限）
>
> [数值] 对于普通文件：链接数，对于目录文件：第一级子目录数
>
> root 第一个为用户名
>
> root 第二个为组名
>
> [数值] 文件大小（单位：字节）
>
> Aug 9 19:37 最后修改时间信息
>
> bin 文件名

#### 设置权限

> chmod 命令
>
> chmod 777 文件名	给当前文件设置为777权限（可随意操作权限）
>
> chmod -R 777 目录名	给当前目录递归权限为777

**权限演示游戏**：[游戏](https://linxz.github.io/tianyizone/linux-chmod-permissions.html)





