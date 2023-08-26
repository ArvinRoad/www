---
title: Java 超级玛丽障碍物类
date: 2021/11/08 14:40:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse1-mm.cn.bing.net/th/id/R-C.8d18ec29ca1875569d6449830649e56f?rik=tlvTzIQRVOByjg&riu=http%3a%2f%2fup.bizhitupian.com%2fpic%2f96%2f7e%2f87%2f967e87e0f1fada27c0d8c56fbde71e5f.jpg&ehk=8LJd6PweRyMat%2fBn01azAQxIOBElfVuhw1wzz3VDEsM%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

#### 代码演示
```java
package com.arvin;

import java.awt.image.BufferedImage;

/**
 *  障碍物类
 */

public class Obstacle implements Runnable{

    // 用于表示当前坐标
    private int x;
    private int y;

    // 用于记录障碍物类型
    private int type;

    // 用于显示图像
    private BufferedImage show = null;

    // 定义当前场景对象
    private BackGround BackGround = null;

    // 定义一个线程对象 用于完成我们旗子下落的过程
    private Thread thread = new Thread(this);

    // 构造函数
    public Obstacle (int x, int y, int type, BackGround BackGround) {
        this.x = x;
        this.y = y;
        this.type = type;
        this.BackGround = BackGround;
        show = StaticValue.Obstacle.get(type);  //  得到该类型的图像

        // 如果是旗子的话 启动线程
        if (type == 4){
            thread.start();
        }
    }

    // 生成上面四个变量的 Get 方法


    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public int getType() {
        return type;
    }

    public BufferedImage getShow() {
        return show;
    }

    // 重写 Runnable 的抽象方法

    @Override
    public void run() {
        while(true){

            if (this.BackGround.isReach()) { // 判断马里奥此时是否到达了旗子的位置
                if (this.y < 374) { // 判断此时的旗子有没有落到地上
                    this.y += 5;
                }else {
                    this.BackGround.setBase(true);
                }
            }

            try {
                Thread.sleep(50);
            } catch (InterruptedException exception) {
                exception.printStackTrace();
            }

        }
    }
}
```