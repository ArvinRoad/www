---
title: kali-Linux 软件管理
date: 2021/9/16 00:50:30
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

## 软件管理

`Lali Linux 系列第三篇`

### Dpkg

> dpkg 是一个 Debian 的一个命令行工具，它可以用来安装、删除、构建和管理 Debian 的软件包

**安装软件**

> dpkg -i xxx.deb

`如果安装出错（缺少依赖），我们需要更新Kali的源`

> apt -get upgrade

> apt --fix-broken install

**卸载软件**

> dpkg -r xxx.deb	--- 删除软件包
>
> dpkg -r --purge xxx.deb	--- 连同配置文件一起删除

### Gdebi

> gdebi 是一个轻量级的 deb 安装工具，它能代替臃肿的 ubuntu软件中心安装 deb

**安装gdebi**

> sudo apt-get update  --- 更新系统
>
> sudo apt-get install gdebi --- 安装软件

**安装软件**

> sudo gdebi xxx.deb 选择Y即可 --- 这个是命令行，当然还包含图形化安装

---

### 压缩命令

**Linux 下常用的压缩文件格式**

> .zip
>
> .gz
>
> .bz2
>
> .tar.gz
>
> .tar.bz2

### tar 命令

**打包命令**

> tar -czvf <打包后的文件名> <源文件名>

**解包命令**

> tar -xzvf <指定解包文件>

**tar 打包时的选项**

>-c	--- 打包
>
>-z	--- 打包同时进行压缩
>
>-v	--- 打包时显示出详细信息（可选）
>
>-f	--- 指定打包文件名

**tar 解包时的选项**

> -x	--- 解包
>
> -z	--- 解包时同时解压
>
> -v	--- 解包时显示出详细信息（可选）
>
> -f	--- 指定解包的文件名

**参考**：[参考文件](https://blog.csdn.net.weixin_38111957/article/details/80210191)

---

### 搜索命令

#### find命令

> find 是通过搜索目录，并输出一个内容

**指定目录查找并输出**

> find ./kali/ -print

**指定目录以 Shell 脚本运行并输出**

> find ./kali/ -exec ls {} \;
>
> find ./kali/ -exec ls -l {} \;	--- -l查看权限信息

**参考**：[参考文件](https://blog.csdn.net/WX_East/article/details/68530883)
