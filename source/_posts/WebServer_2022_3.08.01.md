---
title: WebServer构建
date: 2022/3/08 03:55:00
tags:
  - C语言
  - 教学文档
categories:
  - 程序
cover: https://img.php.cn/upload/article/000/000/020/5e91393a49cb1970.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

注意：该项目是采用C语言调用 WindowsAPI 进行实现的，如果不掌握简单的网络通信协议和C语言与Windows API 编程请先掌握后再来看。
# 什么是协议，理解IP地址和端口的概念
作为新时代标杆的我们，已经离不开手机，离不开网络。对于互联网大家可能耳熟能详，但计算机网络的出现比互联网要早很多。
## 什么是协议
有的人说英语，有的人说中文，有的人说德语，说同一种语言的人可以交流，不同的语言之间就不行了。
如果语言不同则需要翻译，否则两人之间无法沟通，或者约定一下，我们都说英语，这个约定就相当于协议。
网络协议是通信计算机双方必须共同遵从的一组约定。它的三要素是：语法、语义、时序
## 计算机网络沟通用什么
TCP / IP 传输控制协议：是指能够在多个不同网络间实现信息传输的协议簇。
TCP /  IP 协议不仅仅指的是TCP 和 IP 两个协议，而是指一个由 FTP、SMTP、TCP、UDP、IP 等协议构成的协议簇，只是因为 TCP / IP 协议中 TCP 协议和 IP 协议最具有代表性，所以被称为 TCP / IP 协议。
## 端口
### 什么是端口
设备与外界通讯交流的出口。
端口就好比一个房子的门，是出入这间房子的必经之路。

| 物理端口 | 电脑网口、USB |
| --- | --- |
| 虚拟端口 | 程序和网络进行通信的端口 |

### 端口号
端口是通过端口号来标记的，端口号只有整数，范围是从 0 到 65535 2字节存储 2^16 - 1
### 端口是怎样分配的
端口号不是随便使用的，而是按照一定的规定进行分配。
### 知名端口
知名端口就是众所周知的端口号，范围从 0 到 1023
80 端口分配给 HTTP 服务
21 端口分配给 FTP 服务
22 端口分配给 SSH 服务（远程控制服务）
### 动态端口
动态端口的范围是从 1024 到 65535
之所以称为动态端口，是因为它一般不固定分配某种服务，而是动态分配/
动态分配是指当一个系统进程或应用程序进行需要网络通信时，它向主机申请一个端口，主机从可用端口号中分配一个供它使用。
当这个进程关闭时，同时也就释放了所占用的端口号。
# TCP协议
TCP 通讯过程中，必须得先建立相关的链接，才能进行相互通讯，这个类似于我们日常生活中的打电话。TCP 的三次握手。
​如果想让别人能够打通我们的电话获取相应的服务就需要做以下几件事情：

| 买手机 | socket 创建一个套接字 |
| --- | --- |
| 插上手机卡 | bind  绑定ip 和 port（端口号） |
| 设计手机为正常接听状态（即能够响铃） | listen 使套接字变为可以被动链接 |
| 静静的等着别人拨打 | accept 等待客户端的链接 |
| 接收电话 | recv / send 接收发送数据 |

如同上面的电话机过程一样，在程序中，如果想要完成一个 TCP 服务功能，需要的程序流程如上第二列内容。
# HTTP协议
HTTP 协议即超文本传输协议，是Web联网的基础，也是手机联网常用的协议之一，HTTP 协议是建立在 TCP 协议之上的一种应用。
​

### 典型的 HTTP 事务处理有如下的过程
### 客户与服务器建立链接。
客户向服务器提出请求。
服务器接受请求，并根据请求返回相应的文件作为应答。
客户与服务关闭链接。
​

HTTP 请求数据方法：GET HEAD POST PUT 等
每种请求方法规定了客户和服务器之间不同的信息交换方式，常用的请求方法是 GET 和 POST
​

七层模型：物理层，数据层链路层、网络层、传输层、会话层、表示层、应用层。
# 实例代码：
注意：在运行HTML文件前，请在项目属性中 - C/C++ - 常规 - SDL检查改成否
```c
#include <stdio.h>
#include <WinSock2.h>
​

#define PORT 80
#define DATASIZE 1024
​

#pragma comment(lib,"ws2_32.lib")
​

void sendHtml(SOCKET client, char* filenName);
​

int main() {
​

printf("图灵服务器开启成功\n");
/**
*        确定使用的 Socket 版本信息
*        第一个参数：期望使用的版本
*        第二个参数：获取版本信息
*/
WSADATA wsadata;
int ISOK = WSAStartup(MAKEWORD(2, 2), &wsadata);
​

if (ISOK == WSAEINVAL) {
perror("SOCKET 请求失败……\n");
}
/**
*        创建一个 Socket
*        第一个参数：协议族。指定使用什么协议 AF_INET 使用 IPv4
*        第二个参数：传输类型 Sock_STREAM 流传输
*        第三个参数：遵从 TCP 协议
*/
SOCKET server = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
​

if (server == INVALID_SOCKET) {
perror("SOCKET 创建失败……\n");
}
/* 绑定端口号和 IP 地址 */
struct sockaddr_in seraddr;
seraddr.sin_family = AF_INET;
seraddr.sin_port = htons(PORT);        // 注意：网络是大端存储，电脑是小端存储
seraddr.sin_addr.s_addr = INADDR_ANY;
ISOK = bind(server, (struct sockaddr*) & seraddr, sizeof(seraddr));
if (ISOK == SOCKET_ERROR) {
perror("BIND 绑定失败……\n");
}
/* 监听 listen 第二个参数：同时监听多少 */
while (1)
{
printf("等待链接中……\n");
listen(server, 5);
SOCKET client = accept(server, NULL, NULL);
if (client == INVALID_SOCKET) {
perror("ACCEPT 监听失败……\n");
}
/* 接收信息 */
char buffer[DATASIZE];
recv(client, buffer, sizeof(buffer), 0);
//printf("%s 共接收到%d条数据\n", buffer, strlen(buffer));
//char sneddata[DATASIZE] = "<h1 style = \"color:red;\">Hello Turing</h1>";
//send(client, sneddata, strlen(sneddata), 0);
​

/* 网页文件传输 */
sendHtml(client, "./index.html");
​

/* 通信完毕，断开链接 */
closesocket(client);
}
/* 关闭服务器 */
closesocket(server);
/* 关闭 Socket 套接字请求 */
WSACleanup();
​

int num = getchar();
​

return 0;
}
​

void sendHtml(SOCKET client, char* filenName) {
FILE* pfile = fopen(filenName, "r");
if (pfile == NULL) {
perror("HTML 文件打开失败……\n");
return;
}
​

do {
char senddata[DATASIZE];
fgets(senddata, DATASIZE, pfile);
send(client, senddata, strlen(senddata), 0);
} while (!feof(pfile));
​

/* 关闭 */
fclose(pfile);
}
```
