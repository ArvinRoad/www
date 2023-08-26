---
title: Kali-Linux 子域名 Sublist3r
date: 2021/9/25 14:56:50
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

## Kali Linux 子域名 Sublist3r

> **`注意本资料只适用学习研究，禁止进行非授权活动。请熟读网络安全法条规`**

`Lali Linux 系列第四篇信息收集第三章 子域名 Sublist3r`

>**`从几个维度来收集子域名，使用软件进行目录扫描`**

### 子域名收集的作用

>- 扩大渗透测试范围
>- 找到目标站点突破口
>- 业务边界安全
>
>当然泛解析域名是没有利用价值的

**常见子域盲区**

> `子域名打开就是404页面、403页面怎么做`
>
> 案例：**我们如何发现 ucweb.com 的两个 XSS**
>
> 示例：[链接](https://nosec.org/home/detail/2011.html)

### 收集子域名的方法

- **在线收集子域名**

> - **谷歌语法**
>
> 通过特定站点的范围查询子域：site：baidu.com
>
> - **在线爆破**
>
> 在线枚举爆破：[在线枚举爆破链接](http://phpinfo.me/domain/)
>
> - **证书搜索**
>
> 基于 SSL 证书查询子域：[基于 SSL 证书查询链接](https://crt.sh/)
>
> - **DNS 搜索**（比较精准）
>
> 基于 DNS 记录查询子域：[基于 DNS 记录查询子域链接](https://dns.bufferover.run/dns?q=)

- **Fuzzdomain 工具**

> 使用 GitHub 下载相应的子域发现工具（根据文档记得安装依赖）

```v
git clone https://github.com/aboul3la/Sublist3r

# 安装模块（当然它还有一些 DNS 的工具包，可以在说明文档看）
sudo pip3 install -r requirements.txt

# 枚举目标子域
python sublist3r.py -d aqlab.cn

# 枚举子域并且显示开发 80 和 443 端口的子域
python sublist3r.py -d aqlab.cn -p 80,443

# 枚举目标子域并保存
python shublist3r.py -d aqlab.cn -o aqlab.txt
```

- **用户事件**

> - **历史漏洞**
>
> 乌云镜像：[乌云镜像链接](http://www.anquan.us)
>
> - **使用手册、通知**
>
> 学院通知：[示例链接](http://dwz.cn/OOWeYYy6)

---

### 使用御剑扫描敏感目录

> **`御剑是一款很好用的网站后台扫描工具，图形化页面，使用起来简单上手`**
>
> 下载：[字典链接](https://pan.baidu.com/s/1vkwZ390yS0yS2V07MhgECw )
> 下载：[御剑系列链接](https://pan.baidu.com/s/1FAmb9EnuuJm2ogJ6BdYkTg)
> 提取码：YJXZ

**字典查询**

> 弱口令100
>
> Web目录字典  - 域名字典

### robots.txt 文件的作用

>**Robits协议**
>
>（也叫爬虫协议、机器人协议等）全称是 “ 网络爬虫排除标准 ” （Robots Exclusion Protocol）,网站通过 Robots 协议告诉搜索引擎哪些页面可以抓取，哪些页面不能抓取。

> **Robots.txt 文件的重要性**
>
> Robots.txt 是搜索引擎蜘蛛访问网站时要查看的第一个文件，并且会根据 Robots.txt 文件的内容来爬行网站。在某种意义上说，它的一个任务就是指导蜘蛛爬行，减少搜索引擎蜘蛛的工作量。

>**Robots.txt 文件的存放位置**
>
>网站根目录下，通过 “ 域名/robots.txt0 ” 能正常访问即可，如：http://域名/robots.txt

**Robots写法**：[链接](https://baijiahao.baidu.com/s?id=1616368344109675728&wfr=spider&for=pc)