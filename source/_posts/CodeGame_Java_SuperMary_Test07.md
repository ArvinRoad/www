---
title: Java 敌人类
date: 2021/11/14 18:48:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://pic2.zhimg.com/v2-957d2a1fac8856452f3ce0aac00427d9_b.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
### 代码演示

```java
package com.arvin;

import java.awt.image.BufferedImage;

/**
 *  敌人类
 */

public class Enemy implements Runnable {

    // 存储当前坐标
    private int x,y;

    // 存储敌人类型
    private int type;

    // 判断敌人运动的方向
    private boolean face_to = true;

    // 用于显示敌人的当前图像
    private BufferedImage show;

    // 定义一个背景对象
    private BackGround backGround;

    // 食人花运动的极限范围
    private int max_up = 0;
    private int max_down = 0;

    // 定义一个线程对象 实现敌人的运动
    private Thread thread = new Thread(this);

    // 定义当前的图片的状态
    private int image_type = 0;

    /**
     *  蘑菇敌人的构造函数
     */
    public Enemy (int x,int y,boolean face_to,int type,BackGround backGround) {
        this.x = x;
        this.y = y;
        this.face_to = face_to;
        this.type = type;
        this.backGround = backGround;

        show = StaticValue.Mogu.get(0); // 初始化 Show

        thread.start(); // 启动线程

    }

    /**
     *  食人花敌人的构造函数
     */
    public Enemy (int x,int y,boolean face_to,int type,int max_up,int max_down,BackGround backGround){
        this.x = x;
        this.y = y;
        this.face_to = face_to;
        this.type = type;
        this.max_up = max_up;
        this.max_down = max_down;
        this.backGround = backGround;

        show = StaticValue.Flower.get(0);

        thread.start();
    }

    /**
     *  敌人的死亡方法
     */
    public void death () {
        show = StaticValue.Mogu.get(2); // 蘑菇死亡时图片
        this.backGround.getEnemyList().remove(this); //死亡后移除敌人
    }

    /* 生成 x y show 的 get 方法 */

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public BufferedImage getShow() {
        return show;
    }

    // 生成 type的 Get 方法
    public int getType() {
        return type;
    }

    // 抽写 Runnable 接口方法
    @Override
    public void run() {
        while (true){
            /**
             *  判断是否是蘑菇敌人
             */
            if (type == 1) {
                if (face_to){   //判断当前朝向
                    this.x -= 2;
                }else{
                    this.x += 2;
                }
                // 判断切换 敌人的运动图片
                image_type = image_type == 1 ? 0 : 1;
                show = StaticValue.Mogu.get(image_type);
            }
            // 定义两个布尔 它们用于判断的敌人向左还是向右
            boolean canLeft = true;
            boolean canRight = true;

            // 遍历障碍物
            for (int i = 0; i < backGround.getObstacleList().size(); i++) {
                Obstacle obl = backGround.getObstacleList().get(i); // 定义临时变量

                // 判断是否可以向右走
                if (obl.getX() == this.x + 36 && (obl.getY() + 65 > this.y && obl.getY() -35 < this.y)) {
                    canRight = false;
                }

                // 判断是否可以向左走
                if (obl.getX() == this.x - 36 && (obl.getY() + 65 > this.y && obl.getY() -35 < this.y)) {
                    canLeft = false;
                }
            }

            // 判断蘑菇敌人是否向左走 并且已经碰到了我们的障碍物 或者敌人走到了屏幕最左侧
            if (face_to && !canLeft || this.x == 0) {
                face_to = false;    // 重置方向向右走
            }else if((!face_to) && (!canRight) || this.x == 764){ //反过来
                face_to = true;
            }

            /**
             *  判断是否是食人花敌人
             */
            if (type == 2) {
                if (face_to) { // 食人花敌人的运动方向
                    this.y -= 2;
                }else{
                    this.y += 2;
                }
                image_type = image_type == 1 ? 0 : 1;   //图像切换
                // 食人花敌人是否到了上限位置
                if (face_to && (this.y == max_up)) {
                    face_to = false;
                }
                // 食人花敌人是否到了下限位置
                if ((!face_to) && (this.y == max_down)){
                    face_to = true;
                }
                show = StaticValue.Flower.get(image_type);
            }
            try {
                Thread.sleep(50);   // 线程休眠50毫秒
            } catch (InterruptedException exception) {
                exception.printStackTrace();
            }
        }
    }
}
```