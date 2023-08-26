---
title: Error Page 设计演示01
date: 2021/10/31 01:23:52
tags:
  - 设计
categories:
  - Web Page
cover: https://z3.ax1x.com/2021/10/31/5z70Rx.png
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

**分享大家一个好看的 Error Page 代码**

---

### 演示图

![](https://z3.ax1x.com/2021/10/31/5z70Rx.png)

### 演示代码
```html
<!--
    Developer: @Arvin
    Explain: Error Page
-->
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width,initial-scale=1.0"/>
    <title>Error Page</title>
    <style type="text/css">
        /* 导入字体 @Arvin */
        @import url("https://fonts.googleapis.com/css2?family=Bowlby+One+SC&display=swap");

        :root {
            --background-color: #000;
            --border-color    : #7591AD;
            --text-color      : #34495e;
            --color1          : #EC3E27;
            --color2          : #f01d67;
            --color3          : #1474bd;
            --color4          : #04ffcd;
            --color5          : #fdcb6e;
            --color6          : #e056fd;
            --color7          : #F97F51;
            --color8          : #BDC581;
        }

        * {
            margin : 0;
            padding: 0;
        }

        html {
            font-size: 14px;
        }

        body {
            width           : 100vw;
            height          : 100vh;
            background-color: var(--background-color);
            display         : flex;
            justify-content : center;
            align-items     : center;
            font-family     : 'Montserrat', sans-serif, Arial, 'Microsoft Yahei';
        }

        .channel {
            position   : absolute;
            width      : 80%;
            text-align : center;
            top        : 50%;
            left       : 50%;
            transform  : translate(-45%, -200px);
            font-size  : 30px;
            font-weight: bold;
            color      : #fff;
        }

        .box {
            width              : 350px;
            /* background-color: #fff; */

            display        : flex;
            justify-content: space-around;
            align-items    : center;

            transform-style: preserve-3d;
        }

        .box p {
            position        : relative;
            font-size       : 100px;
            font-family     : 'Bowlby One SC', Arial, Helvetica, sans-serif;
            color           : #FFF;
            transform       : scale(0.9, 1) rotateY(-45deg);
            transform-origin: bottom center;
            animation       : animate 2.5s ease-in-out infinite;
            animation-delay: calc(var(--i) * 100ms);
        }

        @keyframes animate {
            20% {
                transform: scale(0.9, 1) rotateY(-45deg);
                text-shadow:
                        0 0 var(--color2),
                        0 0 var(--color3),
                        0 0 var(--color4);
            }

            40% {
                transform: scale(0.9, 2) translateY(16px);
                text-shadow:
                        -10px -2px var(--color2),
                        -20px -3px var(--color3),
                        -30px -4px var(--color4);
            }

            60% {
                transform: scale(0.9, 1) rotateY(45deg);
                text-shadow:
                        0 0 var(--color2),
                        0 0 var(--color3),
                        0 0 var(--color4);
            }

            80% {
                transform: scale(0.9, 2) translateY(16px);
                text-shadow:
                        10px -2px var(--color2),
                        20px -3px var(--color3),
                        30px -4px var(--color4);
            }
        }
    </style>
</head>

<body>
<H1>Error Page</H1>

<div class="channel">
    TCT 呼叫程序猿大大 ＞﹏＜
</div>

<div class="container">
    <div class="box">
        <p style="--i:1">E</p>
        <p style="--i:2">R</p>
        <p style="--i:3">R</p>
        <p style="--i:4">O</p>
        <p style="--i:5">R</p>
    </div>
</div>

<div>
    <div th:Utext="'&lt;!--'" th:remove="tag"></div>
    <div th:utext="'Failed Request URL : ' + ${url}" th:remove="tag"></div>
    <div th:utext="'Expiration message : ' + ${exception.message}" th:remove="tag"></div>
    <ul th:remove="tag">
        <li th:each="st : ${exception.stackTrace}" th:remove="tag"><span th:utext="${st}" th:remove="tag"></span><li>
    </ul>
    <div th:utext="'--&gt;'" th:remove="tag"></div>
</div>
</body>

</html>
```