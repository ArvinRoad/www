---
title: Kali-Linux Nmap 端口扫描
date: 2021/9/21 03:49:50
tags:
  - Kali Linux
  - 教学文档
  - 信息收集
categories:
  - 网安知识
cover: https://tse3-mm.cn.bing.net/th/id/OIP-C.RI-jGvsfW8kRiYJdf5eqlwHaD4?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

## Kali Linux Nmap - 端口扫描

> **`注意本资料只适用学习研究，禁止进行非授权活动。请熟读网络安全法条规`**

`Lali Linux 系列第四篇信息收集第二章 Nmap 端口扫描`

### Nmap 简介

---

> Nmap （" Network Mapper（网络映射器）"）是一款开放源代码的网络探测和安全审核的工具。它的设计目标是快速地扫描大型网络，当然用它扫描单个主机也没有问题。Nmap 以新颖的方式使用原始IP报文来发现网络上有哪些主机，那些主机提供什么服务（应用程序名和版本），那些服务运行在什么操作系统（包括版本信息），它们使用什么类型的报文过滤器 | 防火墙，以及一堆其他功能。虽然 Nmap 通常用于安全审核，许多系统管理员和网络管理员也用它来做一些日常的工作，比如查看整个网络的信息，管理服务升级计划，以及监视主机和服务的运行。

**Nmap 官网**：[网址](www.nmap.org)

---

#### Nmap 的作用

> - 检测存活在网络上的主机（主机发现）
> - 检测主机上开放的端口（端口发现或枚举）
> - 检测到相应的端口（服务发现）的软件和版本
> - 检测操作系统，硬件地址，以及软件版本
> - 检测脆弱性的漏洞（Nmap 的脚本）

---

### Nmap 常用扫描指令

**常用的端口扫描指令**：

> -sS：TCP SYN扫描
>
> -sU：UDP扫描
>
> -sT：TCP扫描
>
> -sV：扫描系统版本和程序版本号检测
>
> -sn：只进行主机发现，使用ping扫描来侦测存活主机不进行端口扫描
>
> -sV：扫描版本号侦测，检测开放端口来判断开放服务，并检测其版本
>
> -sR：RCP扫描 扫描是否为 PRC 端口，存在 PRC 端口则返回程序和版本号
>
> -sP：Ping 扫描
>
> -P0；-Pn：无 Ping 扫描
>
> -PS：TCP SYN Ping 扫描
>
> -PA：TCP ACK Ping扫描
>
> -PU：UDP Ping 扫描
>
> -PE；-PP；-PM：ICMP Ping Types扫描
>
> -PR：ARP Ping 扫描
>
> -p：指定端口号扫描
>
> -PR：ARP ping 扫描 通常在局域网扫描，使用地址解析协议，本地局域网不会禁止 ARP 请求
>
> -sL：列表扫描，列出指定网络上的每台主机，默认使用域名解析获取他们的名字
>
> -v：显示扫描过程
>
> -w：结果的详细输出
>
> -F：快速扫描
>
> -n：禁止反向域名解析
>
> -R：反向域名解析
>
> -6：启动 IPV6 扫描
>
> -o：启动系统探测
>
> -Pn：禁止ping后扫描：跳过主机发现的过程进行端口扫描
>
> -A：全面的系统扫描：包括打开操作系统探测、版本探测、脚本扫描、路径扫描
>
> --scrpt=vuln：全面的漏洞扫描
>
> --allports：全端口版本探测 启用全端口版本扫描，会跳过9100 TCP 段 只有使用 --allports 才能扫描所有的端口
>
> --version-intensity：扫描强度设置 扫描强度为 0-9 ，默认为7，最低为0，最高为9
>
> -system-dns：系统域名解析器，通过直接发送查询到主机上自带配置的域名服务器来解析域名
>
> -traceroute：路由跟踪，帮助用户了解网络情况，可以查出本地计算机到目标之间所经过的网络节点，并可以看到通过各个节点的时间
>
> -PY：SCTP INIT Ping扫描 通过 STCP 协议进行扫描主机
>
> --version-trace：获取详细版本信息 对获取目标主机的额外信息有帮助
>
> --osscan-guess；--fuzzy：探测系统识别 当 nmap 无法准确识别系统时，我们可以用这两个参数进行大胆的尝试猜测系统

**扫描指定IP开放端口**：

> nmap -sS -p <端口号> -v 192.168.1.2
>
> 使用半开扫描，指定端口号1-65535，显示扫描过程

**穿透防火墙扫描**：

> nmap -Pn -A 192.168.1.2
>
> 服务器禁止ping命令，试一试 -Pn，nmap参数配合使用

**保存结果到文件**：

> -Og <文件名>

**漏洞扫描**：

> nmap -script=vuln 192.168.1.2
>
> 使用 vuln 脚本进行全面的漏洞扫描

**指纹识别扫描**：

> nmap -sV -v 192.168.1.2
>
> 扫描系统和程序版本号检测，并输出详细信息

> 在 Root 用户下，直接使用 nmap 目标 启用了 -sS 扫描模式
>
> nmap -p 80-81 192.168.1.2 选择范围扫描
>
> nmap -p 80-81 -Pn 192.168.1.2 穿透防火墙扫描
>
> nmap -p 80-81 -Pn -v 192.168.1.2 显示扫描全过程 
>
> nmap -A -Pn  192.168.1.2 全局扫描

### Nmap 扫描状态

> Opend：端口开启
>
> Closed：端口关闭
>
> Filtered：端口被过滤，数据没有到达主机，返回结果为空，数据被防火墙
>
> Unfiltered：未被过滤，数据有到达主机，但是不能识别端口的当前状态
>
> Open | filtered：开放或者被过滤，端口没有返回值，主要发生在UDP、IP、FIN、NULL 和 Xmas 扫描中
>
> Closed | filtered：关闭或者被过滤，只发生在IP ID idle 扫描

**CVE**:

> http://cve.scap.org.cn/
>
> http://cve.mitre.org/
>
> 信息安全漏洞查询库：我们可以将检测的漏洞编号收集在这里去验证

### Nmap 漏洞扫描

#### 信息收集

> 使用 Nmap 中的脚本进行信息收集

**whois 信息收集**

> 脚本名称：whois-domain.nse
>
> 使用方法：nmap --script whois-domain.nse <目标>
>
> 查询目标 whois 信息

**DNS 解析查询**

> 脚本名称：dns-brute
>
> 使用方法：nmap --script dns-brute <目标>
>
> 通过 DNS 记录进行查询并且进行爆破

**扫描 WEB 漏洞**

> 脚本名称：http:stored-xss.nse
>
> 使用方法：nmap --script http:stored-xss.nse <目标>
>
> 扫描目标是否存在 xss 漏洞
>
> 还有很多 web 扫描脚本

**检测 MySQL 密码**

>  脚本名称：mysql-empty-password
>
> 使用方法：nmap --scirpt=mysql-empty-password <目标>
>
> 检测目标是否存在空口令或者密码为Root

**FTP 服务认证**

> 脚本名称：ftp-brute
>
> 使用方法：nmap --script ftp-brute -p 21 <目标>
>
> 爆破目标是否存在弱口令
>
> 使用字典：nmap --script ftp-burute --script-args user=user.txt,passdword=passdword.txt <目标>

**wordpress认证**

> 脚本名称：http-wordpress-brute
>
> 使用方法：nmap --script http-wordpress-brute
>
> 使用字典：nmap --script http-wordpress-brute --script-args user=user.txt,passdword=passdword.txt <目标>

**Nmap 指定脚本扫描**：

> nmap --script=<指定脚本名> <域名>
>
> nmap --script=smb-vuln-ms17-010 <域名>



### Nmap报告输出

#### 一 . 标准保存

> nmap -oN <文件>.txt <目标域名>
>
> 标准保存会把输出结果保存到指定文件

#### 二 . 保存为 XML 格式

> nmap -oX <文件>.xml <目标域名>
>
> 保存为 XML 格式需要用浏览器打开，查看结果

**社工密码字典在线生成**：[字典链接](www.coolrano.com/caimima/)

