---
title: Java 超级玛丽背景类
date: 2021/11/07 20:13:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse1-mm.cn.bing.net/th/id/R-C.572b4ab9d87462765ef358a9e15819ce?rik=db7288wWpYy4zA&riu=http%3a%2f%2fwww.3dmgame.com%2fuploads%2fallimg%2f160908%2f226_160908015610_1.jpg&ehk=DXhBtkC4y5FgVe%2blsbCvr%2f8wv7AUQFSZwlLPj6DmerM%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
配合文件：[Java 超级玛丽窗口](https://arvinroad.github.io/2021/11/07/CodeGame_Java_SuperMary_Test03/)

---
#### 代码演示：
```Java
package com.arvin;

import java.awt.image.BufferedImage;
import java.util.ArrayList;
import java.util.List;

/**
 *  背景类
 */

public class BackGround {

    /* 调用BufferedImage 该对象用于显示当前场景的图片 */
    private BufferedImage bgImage = null;

    /* 记录当前是第几个场景(关卡) */
    private int sort;

    /* 判断是否是最后一个场景 */
    private boolean flag;

    /* 用于存放我们所有的障碍物 */
    private List<Obstacle> ObstacleList = new ArrayList<>();

    /* 用于存放我们所有的敌人 */
    private List<Enemy> enemyList = new ArrayList<>();

    /* 用于显示旗杆 */
    private BufferedImage Gan = null;

    /* 用于显示城堡 */
    private BufferedImage Tower = null;

    // 判断马里奥是否到达旗杆位置
    private boolean isReach = false;

    // 判断旗子是否落地
    private boolean isBase = false;

    /* 空参构造 */
    public BackGround() {

    }

    /* 含两个参数的构造参数 */
    public BackGround(int sort,boolean flag) {
        this.sort = sort;
        this.flag = flag;

        /* 判断我们此时的 flag 是否为 true */
        if (flag) {
            bgImage = StaticValue.BackgroundImage_Two;
        }else{
            bgImage = StaticValue.BackgroundImage_Noe;
        }

        /* 判断是否是第一关 */
        if (sort == 1) {
            // 绘制第一关的地面，上地面 type = 1，下地面 type = 2
            for (int i = 0; i < 27; i++) {
                ObstacleList.add(new Obstacle(i*30,420,1,this));    // 绘制上地面
            }
            // 双重 For 循环绘制下地面
            for (int j = 0; j <= 120; j += 30) {
                for (int i = 0; i < 27; i++) {
                    ObstacleList.add(new Obstacle(i*30,570-j,2,this));
                }
            }


            /* 绘制砖块A */
            for (int i = 120; i <= 150 ; i += 30) {
                ObstacleList.add(new Obstacle(i,300,3,this));
            }
            // 绘制砖块 B - F
            for (int i = 300; i <= 570; i += 30) {
                if (i == 360 || i == 390 || i == 480 || i==510 || i == 540) {   // 判断是否是蓝色砖块
                    ObstacleList.add(new Obstacle(i,300,3,this));
                }else{  // 否则是普通砖块
                    ObstacleList.add(new Obstacle(i,300,0,this));
                }
            }
            // 绘制砖块 G
            for (int i = 420; i <= 450; i+=30) {
                ObstacleList.add(new Obstacle(i,240,3,this));
            }

            // 绘制水管
            for (int i = 360; i <=600; i += 25){
                if (i == 360){ // 绘制水管最上层
                    ObstacleList.add(new Obstacle(620,i,5,this));
                    ObstacleList.add(new Obstacle(645,i,6,this));
                }else{
                    ObstacleList.add(new Obstacle(620,i,7,this));
                    ObstacleList.add(new Obstacle(645,i,8,this));
                }
            }
            // 绘制第一关蘑菇敌人
            enemyList.add(new Enemy(580,385,true,1,this));
            // 绘制第一关食人花敌人
            enemyList.add(new Enemy(635,420,true,2,328,428,this));
        }

        // 判断是否是第二关
        if (sort == 2) {
            // 绘制第二关地面
            for (int i = 0; i < 27; i++){
                ObstacleList.add(new Obstacle(i*30,420,1,this));
            }
            for (int j = 0; j <= 120; j+= 30){
                for (int i = 0; i < 27; i++){
                    ObstacleList.add(new Obstacle(i*30,570-j,2,this));
                }
            }

            // 绘制水管和第一关一样只需要修改坐标
            for (int i =  360; i <= 600; i += 25){
                if (i == 360) {
                    ObstacleList.add(new Obstacle(60,i,5,this));
                    ObstacleList.add(new Obstacle(85,i,6,this));
                }else{
                    ObstacleList.add(new Obstacle(60,i,7,this));
                    ObstacleList.add(new Obstacle(85,i,8,this));
                }
            }

            for (int i = 330; i <=600; i+= 25){
                if (i == 330) {
                    ObstacleList.add(new Obstacle(620,i,5,this));
                    ObstacleList.add(new Obstacle(645,i,6,this));
                }else{
                    ObstacleList.add(new Obstacle(620,i,7,this));
                    ObstacleList.add(new Obstacle(645,i,8,this));
                }
            }

            // 绘制砖块 C
            ObstacleList.add(new Obstacle(300,330,0,this));

            // 绘制砖块 B E G
            for (int i = 270; i <= 330; i+=30){
                if (i == 270 || i == 330) {
                    ObstacleList.add(new Obstacle(i,360,0,this));
                }else{
                    ObstacleList.add(new Obstacle(i,360,3,this));
                }
            }

            // 绘制砖块 A D F H I
            for (int i = 240; i <= 360; i += 30){
                if (i == 240 || i == 360){
                    ObstacleList.add(new Obstacle(i,390,0,this));
                }else{
                    ObstacleList.add(new Obstacle(i,390,3,this));
                }
            }

            // 绘制妨碍 1 砖块
            ObstacleList.add(new Obstacle(240,300,0,this));

            // 绘制 空 1 - 4 砖块
            for (int i = 360; i <= 540; i += 60) {
                ObstacleList.add(new Obstacle(i,270,3,this));
            }

            // 绘制第二关第一个食人花敌人
            enemyList.add(new Enemy(75,420,true,2,328,418,this));
            // 绘制第二关第二个食人花敌人
            enemyList.add(new Enemy(635,420,true,2,298,388,this));
            // 绘制第二关第一个蘑菇敌人
            enemyList.add(new Enemy(200,385,true,1,this));
            // 绘制第二关第二个蘑菇敌人
            enemyList.add(new Enemy(500,385,true,1,this));
        }

        // 判断是否是第三关
        if (sort == 3) {
            // 绘制第三关地面
            for (int i = 0; i < 27; i++){
                ObstacleList.add(new Obstacle(i*30,420,1,this));
            }
            for (int j = 0; j <= 120; j+= 30){
                for (int i = 0; i < 27; i++){
                    ObstacleList.add(new Obstacle(i*30,570-j,2,this));
                }
            }

            // 绘制 A - O 砖块
            int temp = 290;
            for (int i = 390; i >= 270; i -= 30) {
                for (int j = temp; j <= 410; j += 30) {
                    ObstacleList.add(new Obstacle(j,i,3,this));
                }
                temp += 30;
            }

            // 绘制 P - R 砖块
            temp = 60;
            for (int i = 390; i >= 360; i -= 30) {
                for (int j = temp; j <= 90; j += 30) {
                    ObstacleList.add(new Obstacle(j,i,3,this));
                }
                temp += 30;
            }

            // 绘制旗杆
            Gan = StaticValue.Gan;

            //绘制城堡
            Tower = StaticValue.Tower;

            // 添加旗子到旗杆上 旗子的坐标 x515 y220
            ObstacleList.add(new Obstacle(515,220,4,this));

            // 绘制第三关蘑菇敌人
            enemyList.add(new Enemy(150,385,true,1,this));
        }
    }

    /* 生成上面函数的 Get 方法 */

    public BufferedImage getBgImage() {
        return bgImage;
    }

    public int getSort() {
        return sort;
    }

    public boolean isFlag() {
        return flag;
    }

    // 生成 ObstacleList 的 Get 方法
    public List<Obstacle> getObstacleList() {
        return ObstacleList;
    }

    // 生成 城堡和旗杆的 Get 方法
    public BufferedImage getGan() {
        return Gan;
    }

    public BufferedImage getTower() {
        return Tower;
    }

    // 生成 判断是否到达旗杆位置的 Set Get 方法
    public boolean isReach() {
        return isReach;
    }

    public void setReach(boolean reach) {
        isReach = reach;
    }

    // 生成 判断旗子是否落地 Set Get 方法

    public boolean isBase() {
        return isBase;
    }

    public void setBase(boolean base) {
        isBase = base;
    }

    // 用于存放我们所有的敌人 的 Get 方法
    public List<Enemy> getEnemyList() {
        return enemyList;
    }
}
```