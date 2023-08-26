---
title: Java 创建窗口
date: 2021/11/05 18:57:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse3-mm.cn.bing.net/th/id/OIP-C.mRQUEYkDCy0aqlsBtq54zwHaJg?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

## Java 窗口绘制

> 创建窗口我们需要继承 JFrame 类
>
> 向窗口对象添加键盘监听器 需要该类实现 KeyListener 接口 并重写抽象方法

### 代码演示

```java
package com.arvin;

import javax.swing.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

/**
 *  窗口框架类
 */

public class WindowsFrame  extends JFrame implements KeyListener {

    // 空参构造
    public WindowsFrame(){

        // 设置窗口的大小为 800 * 600
        this.setSize(800, 600);

        // 设置窗口居中显示
        this.setLocationRelativeTo(null);

        // 设置窗口的可见性
        this.setVisible(true);

        // 创建点击窗口上的关闭键，结束程序
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // 设置窗口大小不可变
        this.setResizable(false);

        // 向窗口对象添加键盘监听器 需要该类实现 KeyListener 接口 并重写抽象方法
        this.addKeyListener(this);

        // 设置窗口名称
        this.setTitle("超级玛丽");

    }

    /**
     *  Main 主函数
     */

    public static void main(String[] args){

        // 创建 WindowsFrame 的对象
        WindowsFrame windowsFrame = new WindowsFrame();
    }

    /**
     *  KeyListener并重写抽象方法
     */

    @Override
    public void keyTyped(KeyEvent e) {

    }

    @Override
    public void keyPressed(KeyEvent e) {

    }

    @Override
    public void keyReleased(KeyEvent e) {

    }
}
```

