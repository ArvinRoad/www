---
title: Kali-Linux 搜索引擎 & Google 语法
date: 2021/9/18 00:34:50
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

## Kali Linux 信息收集第一章 搜索引擎 & Google 语法

> **`注意本资料只适用学习研究，禁止进行非授权活动。请熟读网络安全法条规`**

`Lali Linux 系列第四篇信息收集第一章 搜索引擎 & Google 语法`

> 从零开始的信息收集 | **渗透测试的本质就是信息收集也是信息收集的一个过程，以及在过程中我们会使用到的一些技术**

#### 信息收集的作用

> 信息零碎化，收集的工整性（扩大渗透思路）
>
> **最了解你的人，往往都是你的对手。`知己知彼、百战不殆`**

##### 收集哪些信息

**针对 WEB 渗透测试我们需要一下信息**

> - 网站的架构
> - 前端
> - 后端
> - 关于网站的一切信息等

##### 信息收集的重要性

> 知己知彼，百战不殆
>
> 观察、无从下手
>
> 开门有锁 | 需要钥匙
>
> 买U盘的性价比
>
> - 亏 | 存在信息差
> - 赚 | 信息充分利用（优惠价 | 有性价比）
>
> 打错人 | 因为错误信息
>
> 不相关事件 | 蝴蝶效应（学会信息存库）

##### 为什么要进行信息收集

> 获取信息
>
> 了解对方
>
> 掌握情况
>
> 寻找弱点
>
> 安全短板

##### 信息收集的内容

> 对于 Web 渗透测试而言，针对目标系统所相关的信息

| **框架** | 开发人员                              | 安全人员                                        |
| -------- | ------------------------------------- | ----------------------------------------------- |
| 前端     | HTML \| CSS \| JS ···                 | 指纹识别 \| GITHUB/源代码泄露 \| 敏感文件及地址 |
| 后端     | PHP \| ASP .NET \| 容器 \| 数据库···  | 框架识别 \| 容器识别                            |
| 中间件   | 就是中间件                            | 组件报错                                        |
| 系统     | Windows Server \| Linux \| mac Server | 端口 \| 系统识别                                |
| 网络架构 | OSI 模型                              | 域名 \| Whois \| CDN \| C段                     |

---

#### 传统搜索引擎

> 主流搜索引擎分为：百度、Google、360、必应。他们的区别在于收录的结果多少不同。

##### 传统搜索引擎作用

> 传统的搜索引擎能够有效的抓取对方网站页面内容信息
>
> 传统搜索引擎针对网页内容以及网页标题等相关信息进行抓取，提供给我们进行查阅

**可能包含数据**

> - 公司动态
> - 组织文档
> - 用户名 | 密码
> - 测试文件
> - 历史文件

---

#### Google hack 语法

> 常见的 Google 语法，帮助我们快速缩小目标搜索范围

**语法介绍**：

> site：搜索范围限制在某个网站或顶级域名中
>
> inurl：当我们用 inurl 进行查询的时候，Google 会返回那些在 URL（网址）里面包含了我们查询关键字的网页
>
> intext：当我们用 intext 进行查询的时候，Google 会返回那些在文本正文里包含了我们查询关键字的网页
>
> intitle：当我们用 intitle 进行查询的时候，Google 会返回那些在网页标题为查询结果
>
> ···

**Google 语法例子**

> intitle : "netbotz app I i ance" "ok"   --- 搜索标题
>
> inurl : /admin/login.php   --- 搜索地址
>
> inurl : qq.txt  --- 搜索文件 
>
> filetype : xls "username | password" --- 搜索文件类型
>
> intext : password "Login Info" filetyoe : txt --- 搜索文件
>
> filetype:xls"身份证" --- 搜索文件类型

**扩展思路**：[扩展](https://www.exploit-db.com/google-hacking-database)（Google 白帽漏洞数据库）

**Google 镜像**

> https://www.ggjxz.top/
>
> https://gg456.top/
>
> https://tools.bugscaner.com/google/

---

#### 网络空间引擎

> 基于物联网搜索，搜索联网的网络设备（如打印机、摄像头、智能家居）
>
> 扫描在线的暴露的网络设备：路由器、主机、智能电视、联网设备

**网络空间搜索引擎**

> - 钟馗之眼：http://www.zoomeye.org
> - Shodan：https://www.shodan.io
> - fofa：https://fofa.so/
> - 傻蛋：https://www.oshadan.com/
> - Dnsdb 搜索：https://www,dnsdb.io/zh-cn/

---

#### 精细化搜索

> 微信公众号：https://weixin.sogou.com/
>
> 知乎相关：https://www.zhihu.com/search?q=
>
> 微博相关：http://s.weibo.com/?Refer=
>
> 购物：https://search.jd.com/Search?enc=utf-8&keyword
>
> github：https://github.com/search?q=
>
> 贴吧：http://tieba.baidu.com/f/search/res?qw=

> 例如：
>
> site:tieba.baidu.com "qq号"
>
> weixin.sougou.com  --- 搜狗微信文章，文章有试用的账户密码等

---

#### 撰写信息收集报告

**整理信息的方式有很多种，我们的最终目标是整理我们收集到的信息进行分析**

> **一 . 信息收集-环境**
>
> 域名：
>
> 脚本语言：
> 数据库：
>
> 中间件：
>
> 服务器系统：
>
> WAF：无
>
> CMS 或 框架：
>
> 真实 IP：
>
> 开放的敏感端口：
>
> 历史解析 IP：
>
> 注册邮箱：
>
> Whois：
>
> 安全措施（如：自定义404、伪静态处理、流量防护等）.. 404
>
> 子域名：（如：自定义404、伪静态处理、流量防护等）
>
> 旁站：
>
> 目录信息：
>
> ····
>
> **二 . 信息收集 - 网络架构**
>
> 1. 搜索引擎收集目标信息（site + 找后台语法）
>
>    `外围式信息收集，不经过目标。可避免各种环境因素干扰。应当首选。`
>
>     1.1 百度
>
>    ​	结果：无可利用
>
>      1.2 Google
>
>    ​	结果：无可利用
>
>      1.3 必应
>
>    ​    结果：无可利用
>
>      1.4 sougou
>
>       结果：无可利用
>
>      1.5 360 搜索
>
>       结果：无可利用
>
> 2. 目标爬行
>
>    `爬行相对于暴力扫描来说动静小很多、且对于目标的收集效率高`
>
>    注：可使用 Burp suite、Awvs
>
>    结果：
>
>    
>
> 3. 暴力猜解
>
>     3.1 基于字典的扫描。
>
>    `注：可疑目录继续进行子目标扫描..`
>
>    结果：
>
>    
>
> **三 . 弱点探测**
>
> 1. 平台漏扫（+蜘蛛 UA）
>
>    Robots.txt

> 域名：xxx
>
> 子域名：xxx
>
> ​			  xxx
>
> 脚本语言：xxx
>
> ···

`还可以通过表格,找到适合自己的信息整理方式。我个人喜欢表格`

---

**扩展**：

> 相应工具有相应的功能
>
> - 通过端口扫描搜索数据库信息、端口信息
> - 域名可以查IP、查旁站、查后台