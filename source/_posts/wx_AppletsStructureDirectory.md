---
title: 微信小程序结构目录
date: 2021/10/8 18:43:50
tags:
  - 微信小程序
  - 教学文档
categories:
  - 程序
cover: https://img.php.cn/upload/article/000/000/040/5e7980c17f305440.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

## 微信小程序结构目录
> **微信小程序框架**的目标是通过尽可能简单、高效的方式让开发者可以在微信中开发具有原生 App 的体验的服务。
>
> 小程序框架提供了自己的视图层描述语言 **WXML** 和 **WXSS** ,以及 **JavaScript** ，并在视图层与逻辑层间提供了数据传输和事件系统，让开发者能够专注于数据与逻辑。

---

### 小程序文件结构和传统 Web 对比

| 结构 |  传统 Web  | 微信小程序  |
| :--: | :--------: | :---------: |
| 结构 |    HTML    |    WXML     |
| 样式 |    CSS     |    WXSS     |
| 逻辑 | JavaScript | Java script |
| 配置 |     无     |    JSON     |

通过以上对比得出，**传统 Web** 是三层结构。而微信小程序是四层结构，多了一层 **配置.json**

---

### 基本的项目目录

![微信小程序结构目录](https://z3.ax1x.com/2021/10/08/5P3C9A.png)

