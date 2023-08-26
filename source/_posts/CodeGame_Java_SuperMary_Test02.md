---
title: Java 超级玛丽常量类
date: 2021/11/06 19:44:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse1-mm.cn.bing.net/th/id/R-C.561cca1ae61021b1003bcd9bfc912a54?rik=Ma3%2f0x90xOBW9A&riu=http%3a%2f%2fpic109.nipic.com%2ffile%2f20160912%2f11443495_131824907000_2.jpg&ehk=Gjmj3gOeXe1F6b%2bKGcs9g7CJRaZOO7NcU0i0l3%2bVSNQ%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
```java
package com.arvin;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

/**
 *  常量类 (初始化图片)
 */

public class StaticValue {

    /**
     *  定义所需的所有变量
     */

    public static BufferedImage BackgroundImage_Noe = null; // 背景 1
    public static BufferedImage BackgroundImage_Two = null; // 背景 2
    public static BufferedImage Jump_Left = null;   // 马里奥向左跳
    public static BufferedImage Jump_Right = null; // 马里奥向右跳
    public static BufferedImage Stand_Left = null;  // 马里奥向左站立
    public static BufferedImage Stand_Right = null; // 马里奥向右站立
    public static BufferedImage Tower = null;   // 城堡
    public static BufferedImage Gan = null; // 旗杆

    /* 障碍物 因为障碍物很多 所以我们要创建一个列表 */
    public static List<BufferedImage> Obstacle = new ArrayList<>();

    /* 马里奥向左跑 是两张图片所以也要创建一个列表 */
    public static List<BufferedImage> Run_Left = new ArrayList<>();
    public static List<BufferedImage> Run_Right = new ArrayList<>(); // 马里奥向右边跑

    /* 蘑菇敌人 两张行动图像一张死亡图像所以也要创建一个列表 */
    public static List<BufferedImage> Mogu = new ArrayList<>();

    /* 食人花敌人 两种图片一个张嘴图一个闭嘴图所以也要创建一个列表 */
    public static List<BufferedImage> Flower = new ArrayList<>();

    /* 路径的前缀 方便后续调用 定义一个Path 我们通过绝对路径来获取图片的路径我们除了名称外前缀都是一样的所以我们把它定义成一个变量 */
    public static String Path = System.getProperty("user.dir") + "/src/images/";

    /* 初始化方法 */
    public static void Init(){

        // 加载背景图片 try捕获 read 异常
        try {
            BackgroundImage_Noe = ImageIO.read(new File(Path + "bg.png"));      //  背景 1
            BackgroundImage_Two = ImageIO.read(new File(Path + "bg2.png"));     //  背景 2、
            Jump_Left = ImageIO.read(new File(Path + "s_mario_jump1_L.png"));   //  马里奥向左跳跃
            Jump_Right = ImageIO.read(new File(Path + "s_mario_jump1_R.png"));  //  马里奥向右跳跃
            Stand_Left = ImageIO.read(new File(Path + "s_mario_stand_L.png"));  //  马里奥向左站立
            Stand_Right = ImageIO.read(new File(Path + "s_mario_stand_R.png")); //  马里奥向右站立
            Tower = ImageIO.read(new File(Path + "tower.png"));                 //  加载城堡
            Gan = ImageIO.read(new File(Path +"gan.png"));                      //  加载旗杆
        } catch (IOException exception) {
            exception.printStackTrace();
        }

        /* 加载障碍物 try捕获 read 异常 */
        try {
            Obstacle.add(ImageIO.read(new File(Path + "brick.png")));       // 首先加载砖块一种是可破坏的一种是不可破坏的
            Obstacle.add(ImageIO.read(new File(Path + "soil_up.png")));     // 加载地面(上地面) 分为两种一个是我们的上地面一个是我们的下地面
            Obstacle.add(ImageIO.read(new File(Path + "soil_base.png")));   // 加载下地面
        } catch (IOException exception) {
            exception.printStackTrace();
        }

        /* 加载不可破坏的砖块和旗子 try捕获 read 异常 */
        try {
            Obstacle.add(ImageIO.read(new File(Path + "brick2.png")));  //  不可破坏砖块
            Obstacle.add(ImageIO.read(new File(Path + "flag.png")));    //  旗子障碍物
        } catch (IOException exception) {
            exception.printStackTrace();
        }

        /* 加载水管 因为是四个图像所以我们采用 For 循环处理 try捕获 read 异常 */
        for (int i = 1; i <= 4; i++) {
            try {
                Obstacle.add(ImageIO.read(new File(Path + "pipe" + i + ".png")));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
        }

        /* 加载马里奥向左跑图片 因为有两张所以我们要采用 For 循环 try捕获 read 异常 */
        for (int i = 1; i <= 2; i++) {
            try {
                Run_Left.add(ImageIO.read(new File(Path + "s_mario_run" + i + "_L.png")));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
        }

        /* 加载马里奥向右跑图片 因为有两张所以我们和加载马里奥向左跑图片一样 */
        for (int i = 1; i <= 2; i++) {
            try {
                Run_Right.add(ImageIO.read(new File(Path + "s_mario_run" + i + "_R.png")));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
        }

        /* 加载蘑菇敌人 因为是三张图片所以我们仍然通过一个 For 循环 */
        for (int i = 1; i <= 3; i++) {
            try {
                Mogu.add(ImageIO.read(new File(Path + "fungus" + i + ".png")));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
        }

        /* 加载食人花敌人 因为是两张图片所以我们仍然通过一个 For 循环 加载*/
        for (int i = 1; i <= 2; i++){
            try {
                Flower.add(ImageIO.read(new File(Path + "flower1." + i + ".png")));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
        }

    }
}
```