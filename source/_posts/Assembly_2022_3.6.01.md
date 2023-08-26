---
title: 初识汇编
date: 2022/3/06 17:37:00
tags:
  - 汇编
  - 教学文档
categories:
  - 程序
cover: https://tse1-mm.cn.bing.net/th/id/R-C.713f092ccdb317f1f0c087f3539f7cab?rik=bOE%2bVTWSO1ntvA&riu=http%3a%2f%2fi1.hdslb.com%2fbfs%2farchive%2fde6d0f9a0cc0e1ccae53a428089163f10b795a9d.jpg&ehk=7dDrxNiIDQOVlzPnHk42gtOex6jz9ANqVywfOJIg2lk%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

汇编语言是用计算机的思维去操作计算机。

# 汇编语言概述
汇编语言是直接在硬件上工作的编程语言，首先要了解硬件系统的结构（重点主要在：CPU与内存），才能有效的应用汇编语言对其编程。
汇编学习的重点在如何利用硬件系统的编程结构和指令集有效灵活的控制系统进行工作。
## 机器语言

- 机器语言是机器指令的集合。
- 机器指令展开来讲就是一台机器可以正确执行的命令。

**指令**：01010000（PUSH AX）把X推进堆栈
**电平脉冲**：表示电子信号的浮点。0为平1为凸。
早期的程序员将0、1数字编程的程序代码打在纸带或卡片上，1打孔、0不打孔，再将程序通过纸带机或卡片机输入计算机，进行运算。后来，逐渐使用高科技（继电器、晶体管、石英震动），但打孔是始祖。
**例如**：S =  768 + 12288 - 1280
**机器码**：101100000000000000000011000001010000000000110000001011010000000000000101
假如将程序错写成以下，请找出错误：101100000000000000000011000001010000000001010000010110100000000000000101
如果在显示器上输出：welcome to masn。如果使用机器码。你看到这样的程序会怎么想？如果程序里有一个1被误写成0又如何去查找呢？
## 汇编语言的产生
汇编语言的主体是汇编指令。汇编指令和机器指令的差别在于指令的表示方法上。汇编指令是机器指令便于记忆的书写格式。汇编指令是机器指令的助记符。
例如：机器指令1000100111011000表示寄存器BX的内容送到AX中。汇编指令是MOV AX,BX（BX的移动到AX）。这样的写法与人类语言接近，便于阅读和记忆。
计算器能读懂的只有机器指令，那么如何让计算机执行程序员用汇编指令编写的程序呢？汇编语言会通过编译器（转化成机器码）输入进机器就可以执行了。
## 汇编语言的组成
汇编语言由以下三类组成：

- 汇编指令（机器码的助记符）
- 伪指令（由编译器执行）
- 其他符号（由编译器识别）

汇编语言的核心是汇编指令，它决定了汇编语言的特性。
## 指令和数据
指令和数据是应用上的概念。在内存或磁盘上，指令和数据没有任何区别，都是二进制信息。
**二进制信息**：
1000100111011000 -> 89D8H（数据）
1000100111011000 -> MOV AX,BX（程序）
**什么是内存地址空间？**
一个CPU的地址宽度为10，那么可以寻址1024个内存单元，这1024个可寻到的内存单元就构成这个CPU的内存地址空间。首先要介绍两个部分的基础知识，主板和接口卡（**请跳转至各类存储器的芯片，看完后再向下看**）。
上述的那些存储器在物理上是独立的器件。但它们在以下两点上相同：

- 都和CPU总线相连
- CPU对它们进行读或写的时候都通过控制线发出内存读写命令。（CPU读写时候看不到内存，看主存储器地址空间的那一类：[CPU读写时看到的图片演示](https://onedrive.live.com/sync?ru=https%3A%2F%2Fd.docs.live.net%2F1a76aba5568fb83d%2FOneNote%2520%E4%B8%8A%E4%BC%A0%2F%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80#t=EgAkAgMAAAAMgAAADQABIF3g6JPKWfrdh6f0xNL4C5UltnAOXkUw4812GSMAt2%2BHz3HcBWdTB%2Br%2F9mrbeHmy1SlRh5Wszh5Fxun%2BKyFm%2BR%2BdTxupzJF9HxGyD0zZO2NoLrSv%2Fw1QMQdOY0SYBTSBv2sCwJ4LY%2FH6E2eap92DJi1%2BAn0hC%2FcTZfcULl6U6CyvQbXKFGFWk2YdWGmZz1Xk4VdGCQbx5paThA6osOnty10YnRjtuaboXZjXAAINh7rixrZzFgLzIcY6D7mB1xJ8R0LxhkmsBqU7u%2BQi5590hZEYOQ638FphMQr8F6tP%2B%2Fp7Xsf01g9klDDSepJK7zUUKfMSVOBP9cJyzXBBrmKRbhMBewATAf2%2FAwA2VvsG%2FNkXYQf6%2BGAQJwAAChOgAAASADI2NDQyNjY2NTZAcXEuY29tAFsAAB4yNjQ0MjY2NjU2JXFxLmNvbUBwYXNzcG9ydC5jb20AAAAsQ04ABjIzMDAwMAAAdvAIBAIAAJGQbUAwBkEABUFydmluAARSb2FkBMgAAAAAAAAAAAAAAAAAAAAAVo%2B4PRp2q6UAAPzZF2G6LY5hAAAAAAAAAAAAAAAAEAAyMjMuMTA0LjIwMi4yMTgABQQAAAADQBgA7QaS8AQQBQAAAAAAAAAAAAAAAAAAAAhIY8RQXPufA0AYAHcikvAEABgARP9T1wAAAAAAAAAAAAAAAAAA%2Fz8jAIqnb4UAAAAAAwA%3D)

**假设**：上图中的内存空间地址段分配如下：

- 地址0~7FFFH的32KB空间为主随机存储器的地址空间。
- 地址8000H~9FFFH的8KB空间为显存地址空间。
- 地址A000H~FFFFH的24kb空间为各个ROM的地址空间。

**不同的计算机系统的内存地址空间分配情况是不同的。（我们这里采用的是英特尔系列的架构）：**[8086PC机的内存地址空间分配](https://onedrive.live.com/sync?ru=https%3A%2F%2Fd.docs.live.net%2F1a76aba5568fb83d%2FOneNote%2520%E4%B8%8A%E4%BC%A0%2F%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80%2F8086PC%E6%9C%BA%E7%9A%84%E5%86%85%E5%AD%98%E5%9C%B0%E5%9D%80%E7%A9%BA%E9%97%B4%E5%88%86%E9%85%8D.png#t=EgAkAgMAAAAMgAAADQABIF3g6JPKWfrdh6f0xNL4C5UltnAOXkUw4812GSMAt2%2BHz3HcBWdTB%2Br%2F9mrbeHmy1SlRh5Wszh5Fxun%2BKyFm%2BR%2BdTxupzJF9HxGyD0zZO2NoLrSv%2Fw1QMQdOY0SYBTSBv2sCwJ4LY%2FH6E2eap92DJi1%2BAn0hC%2FcTZfcULl6U6CyvQbXKFGFWk2YdWGmZz1Xk4VdGCQbx5paThA6osOnty10YnRjtuaboXZjXAAINh7rixrZzFgLzIcY6D7mB1xJ8R0LxhkmsBqU7u%2BQi5590hZEYOQ638FphMQr8F6tP%2B%2Fp7Xsf01g9klDDSepJK7zUUKfMSVOBP9cJyzXBBrmKRbhMBewATAf2%2FAwA2VvsG%2FNkXYQf6%2BGAQJwAAChOgAAASADI2NDQyNjY2NTZAcXEuY29tAFsAAB4yNjQ0MjY2NjU2JXFxLmNvbUBwYXNzcG9ydC5jb20AAAAsQ04ABjIzMDAwMAAAdvAIBAIAAJGQbUAwBkEABUFydmluAARSb2FkBMgAAAAAAAAAAAAAAAAAAAAAVo%2B4PRp2q6UAAPzZF2G6LY5hAAAAAAAAAAAAAAAAEAAyMjMuMTA0LjIwMi4yMTgABQQAAAADQBgA7QaS8AQQBQAAAAAAAAAAAAAAAAAAAAhIY8RQXPufA0AYAHcikvAEABgARP9T1wAAAAAAAAAAAAAAAAAA%2Fz8jAIqnb4UAAAAAAwA%3D)
**内存地址空间**：最终运行程序的是CPU，我们用汇编（**所有编程，这是学习编程的核心思维**）编程的时候，必须要从CPU角度考虑问题。对CPU来讲，系统中的所有存储器中的存储单元都处于一个统一的逻辑存储器中，它的容量受CPU寻址能力的限制。这个逻辑存储器即是我们所说的内存地址空间。
​

# 硬件说明
**寄存器**：简单的讲是CPU中可以存储数据的器件，一个CPU中有多个寄存器。AX是其中一个寄存器的代号，BX是另一个寄存器的代号。
**存储器：**CPU是计算机的核心部件，它控制整个计算机的运作并进行运算，想要让一个CPU工作，就必须向它提供指令和数据（指令是怎么去做，数据是那些是做的那些是被做的）。指令和数据在存储器中存放，也就是平时说的内存。在一台PC机中内存的作用仅次于CPU，离开了内存，性能再好的CPU也无法工作，磁盘不同于内存，磁盘上的数据或程序如果不读到内存中，就无法被CPU使用。
**存储单元**：存储器被划分为若干个存储单元，每个存储单元从0开始顺序编号。例如：一个存储器有128个存储单元，编号从0~127。对于大容量的存储器一般还用以下单位来计量容量（以下用B来代表Byte）：1KB = 1024B \ 1MB = 1024KB \ 1GB = 1024MB \ 1TB = 1024GB。磁盘的容量单位同内存的一样，实际上以上单位是微机中常用的计量单位。
**主板：**在每一台PC机中，都有一个主板，主板上有核心器件和一些主要器件。这些器件通过总线（地址总线、数据总线、控制总线）相连。
**接口卡**：计算机系统中，所有可用程序控制其工作的设备，必须受到CPU的控制。CPU对外部设备不能直接控制，如显示器、音响、打印机等。直接控制这些设备进行工作的是插在扩展插槽上的接口卡。
## 各类存储器的芯片
从读写属性上看分为两类：随机存储器（RAM）和只读存储器（ROM）。内存就是一个大的随机存储器，特性是断电数据会遗失。只读存储器永远只能读，但是数据是永久存储的。只读存储器装有BIOS的ROM，BIOS：Basic Input/Ouutput System，基本输入输出系统。BIOS是由主板（显卡等也有）和各类接口卡（如：显卡、网卡等）厂商提供的软件系统，可以通过它利用该硬件设备进行最基本的输入输出。在主板和某些接口卡上插有存储相应BIOS的ROM。
从功能和连接分类：

- 随机存储器RAM
- 装有BIOS的ROM
- 接口卡上的RAM

PC机中各类存储器的逻辑连接情况：[PC机中各类存储器的逻辑连接情况演示图](https://onedrive.live.com/sync?ru=https%3A%2F%2Fd.docs.live.net%2F1a76aba5568fb83d%2FOneNote%2520%E4%B8%8A%E4%BC%A0%2F%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80%2FPC%E6%9C%BA%E4%B8%AD%E5%90%84%E7%B1%BB%E5%AD%98%E5%82%A8%E5%99%A8%E7%9A%84%E9%80%BB%E8%BE%91%E8%BF%9E%E6%8E%A5%E6%83%85%E5%86%B5.png#t=EgAkAgMAAAAMgAAADQABIF3g6JPKWfrdh6f0xNL4C5UltnAOXkUw4812GSMAt2%2BHz3HcBWdTB%2Br%2F9mrbeHmy1SlRh5Wszh5Fxun%2BKyFm%2BR%2BdTxupzJF9HxGyD0zZO2NoLrSv%2Fw1QMQdOY0SYBTSBv2sCwJ4LY%2FH6E2eap92DJi1%2BAn0hC%2FcTZfcULl6U6CyvQbXKFGFWk2YdWGmZz1Xk4VdGCQbx5paThA6osOnty10YnRjtuaboXZjXAAINh7rixrZzFgLzIcY6D7mB1xJ8R0LxhkmsBqU7u%2BQi5590hZEYOQ638FphMQr8F6tP%2B%2Fp7Xsf01g9klDDSepJK7zUUKfMSVOBP9cJyzXBBrmKRbhMBewATAf2%2FAwA2VvsG%2FNkXYQf6%2BGAQJwAAChOgAAASADI2NDQyNjY2NTZAcXEuY29tAFsAAB4yNjQ0MjY2NjU2JXFxLmNvbUBwYXNzcG9ydC5jb20AAAAsQ04ABjIzMDAwMAAAdvAIBAIAAJGQbUAwBkEABUFydmluAARSb2FkBMgAAAAAAAAAAAAAAAAAAAAAVo%2B4PRp2q6UAAPzZF2G6LY5hAAAAAAAAAAAAAAAAEAAyMjMuMTA0LjIwMi4yMTgABQQAAAADQBgA7QaS8AQQBQAAAAAAAAAAAAAAAAAAAAhIY8RQXPufA0AYAHcikvAEABgARP9T1wAAAAAAAAAAAAAAAAAA%2Fz8jAIqnb4UAAAAAAwA%3D)

# CPU对存储器读和写（核心）
**CPU要想进行数据的读写，必须和外部器件（标准的说法是芯片）进行三类信息交互：**

- 存储单元的地址（地址信息）
- 器件的选择，读或写命令（控制信息）
- 读或写的数据（数据信息）

**那么CPU是通过什么将地址、数据和控制信息传到存储芯片中呢？**
电子计算机能处理、传输的信息都是电信号，电信号当然用导线传输。在计算机中专门有连接CPU和其他芯片的导线，通常称为总线。物理上：是一根根导线的集合。
逻辑上划分为：

- 地址总线
- 数据总线
- 控制总线

**CPU在内存中是如何读或写的？**

- 读：CPU通过地址总线向内存发送信号，找到地址后，CPU通过控制总线进行发送指令，内存接受到CPU控制总线指令后通过数据总线将数据信息发送到CPU。
- 写：CPU通过地址总线向内存发送信号，找到地址后，CPU通过控制总线进行发送指令，CPU将数据信息通过数据总线写入内存中。

从上面我们知道CPU是如何进行数据的读写，**可是我们如何命令计算机进行数据的读写？**
1000100111011000 -> 89D8H（数据）
1000100111011000 -> MOV AX,BX（程序）
数据有时候可以代表程序，区分的方式就是看数据是从哪个总线传给CPU。
**案例：**
对于8086CPU，下面的机器码能够完成从3号单元读数据：

- 机器码：101000000000001100000000。
- 含义：从3号单元读取数据送入寄存器AX。
- CPU接收这条机器码后将完成上面所述的读写工作。

**地址总线**：CPU是通过地址总线来指定存储单元的。地址总线上能传送多少个不同的信息，CPU就可以对多少个存储单元进行寻址。
64Bit就代表64个总线：当CPU和系统以及软件都是64位才能达到64位寻址速度，缺一不可。那么，地址总线如何发送地址信息呢？
CPU向内存发送0000001011后内存会定位1011地址。
一个CPU有N根地址总线，则可以说找个CPU的地址总线的宽度为N。这样的CPU最多可以寻找2的N次方个内存单元。
**数据总线**：CPU与内存或其他器件之间的数据传送是通过数据总线来进行的，数据总线的宽度决定了CPU和外界的数据传送速度。
**控制总线**：CPU对外部器件的控制是通过控制总线来进行的。在这里控制总线是个总裁，控制总线是一些不同控制线的集合。有多少根控制总线，就意味着CPU提供了对外部器件的多少种控制。所以控制总线的宽度决定了CPU对外部器件的控制能力。前面所说的内存读写命令是由几根控制线综合发出的：

- 其中一根名为**读信号**输出控制线负责由CPU向外传送读信号，CPU向改控制线上**输出低电平**表示将读取数据。
- 有一根名为**写信号**输出控制线负责由CPU向外传送写信号。

​

# 总结

- 汇编指令是机器指令的助记符，同机器指令一一对应。
- 每一种CPU都有自己的汇编指令集。
- CPU可以直接使用的信息在存储器中存放。
- 在存储器中指令和数据没有任何区别，都是二进制信息。
- 存储单元从零开始顺序编号。
- 一个存储单元可以存储8个bit（用作单位写成b），即8位二进制数。
- 1B = 8b \ 1KB = 1024B \ 1MB = 1024KB \ 1GB = 1024MB \ 1TB = 1024GB。
- 每个CPU芯片都有许多管脚，这些管脚和总线相连。也就是说，这些管脚引出总线。一个CPU可以引出三种总线的宽度标志了这个CPU不同方面的性能：
   - 地址总线的宽度决定了CPU的寻址能力。
   - 数据总线的宽度决定了CPU与其他器件进行数据传送时的一次数据传送量。
   - 控制总线决定了CPU对系统中其他器件的控制能力。
