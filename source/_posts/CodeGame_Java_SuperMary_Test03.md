---
title: Java 超级玛丽窗口
date: 2021/11/07 20:16:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse1-mm.cn.bing.net/th/id/OIP-C.AEFItfuco5DjCukU9F-f-wFOC6?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
#### 说明
首先要基于：[Java 创建窗口](https://arvinroad.github.io/2021/11/05/CodeGame_Java_SuperMary_Test01/)

其次基于：[Java 超级玛丽常量类](https://arvinroad.github.io/2021/11/06/CodeGame_Java_SuperMary_Test02/)

搭配文件：[Java 超级玛丽背景类](https://arvinroad.github.io/2021/11/07/CodeGame_Java_SuperMary_Test04/)

---
#### 代码演示
```java
package com.arvin;

import javazoom.jl.decoder.JavaLayerException;

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;

/**
 *  窗口框架类
 */

public class WindowsFrame  extends JFrame implements KeyListener,Runnable {

    // 用于存储所有的背景
    private List<BackGround> AllBackground = new ArrayList<>();

    // 用于存档当前的背景
    private BackGround NowBackground = new BackGround();

    // 用于双缓存
    private Image OffScreenImage = null; // 该变量为我们的双缓存

    // 定义马里奥对象
    private Mario mario = new Mario();

    // 定义一个线程对象用于马里奥运动
    private Thread thread = new Thread(this);

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

        // 在空参构造中调用常量类的初始化(图片)方法
        StaticValue.Init();

        // 初始化马里奥对象
        mario = new Mario(10,355);

        // 创建全部场景，因为有3个场景所以我们要通过 For 循环进行创建
        for (int i = 1; i <= 3; i++) {
            AllBackground.add(new BackGround(i, i == 3 ? true : false)); // 传入二个参数：当前关卡数 | 是否为最后一关的布尔变量(三目运算) |
        }

        // 将第一个场景设置为当前场景，修改这里参数可以进入不同关卡
        NowBackground = AllBackground.get(0);
        mario.setBackGround(NowBackground); // 将第一个场景的 BackGround 对象传递给马里奥

        // 调用 repaint(); 方法 绘制图像
        repaint();

        thread.start(); // 启动线程

        // 播放音乐 实例化一个 Music 对象
        try {
            new Music();
        } catch (FileNotFoundException exception) {
            exception.printStackTrace();
        } catch (JavaLayerException exception) {
            exception.printStackTrace();
        }

    }

    /**
     *  重写 Paint 方法
     */
    public void paint(Graphics GraphicsPaint) {
        if (OffScreenImage == null){
            OffScreenImage = createImage(800,600);  // 为空 创建图像 调用 createImage() 方法
        }

        Graphics graphics = OffScreenImage.getGraphics(); // 定义 Graphics 对象
        graphics.fillRect(0,0,800,600); // 调用 Graphics 的 fillRect(); 方法对我们的图像进行填充
        graphics.drawImage(NowBackground.getBgImage(),0,0,this); // 绘制背景 (此时我们的图像绘制在缓冲区上)
        // 绘制敌人 (注意绘制在障碍物前，因为要被障碍物挡住)
        for (Enemy enemy : NowBackground.getEnemyList()){
            graphics.drawImage(enemy.getShow(),enemy.getX(),enemy.getY(),this);
        }

        // 绘制障碍物 通过增强 For 循环
        for (Obstacle obstacle:NowBackground.getObstacleList()) {
            graphics.drawImage(obstacle.getShow(),obstacle.getX(),obstacle.getY(),this);
        }

        // 绘制城堡
        graphics.drawImage(NowBackground.getTower(),620,270,this);

        // 绘制旗杆
        graphics.drawImage(NowBackground.getGan(),500,220,this);

        // 绘制马里奥对象
        graphics.drawImage(mario.getShow(),mario.getX(),mario.getY(),this);

        // 绘制积分
        Color color = graphics.getColor(); // 保存我们的颜色
        graphics.setColor(Color.BLACK); // 颜色设置为黑色
        graphics.setFont(new Font("宋体",Font.BOLD,25)); // 设置字体为宋体 加粗 字号 25
        graphics.drawString("当前分数："+ mario.getScore(),300,100); // 绘制显示
        graphics.setColor(color); // 还原我们的字体颜色

        GraphicsPaint.drawImage(OffScreenImage,0,0,this); // 我们将缓冲区的图片绘制到窗口中
        
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

    // 当键盘按下按钮时调用
    @Override
    public void keyPressed(KeyEvent e) {

        // 向右移动
        if (e.getKeyCode() == 39) {
            mario.rightMove();
        }

        // 向左移动
        if (e.getKeyCode() == 37) {
            mario.leftMove();
        }

        // 跳跃
        if (e.getKeyCode() == 32){
            mario.jump();
        }

    }

    // 当键盘松开按键时调用
    @Override
    public void keyReleased(KeyEvent e) {

        // 向左停止
        if (e.getKeyCode() == 37){
            mario.leftStop();
        }

        // 向右停止
        if (e.getKeyCode()  == 39){
            mario.rightStop();
        }

    }

    // 重写 Runnable 抽象方法
    @Override
    public void run() {
        while (true){
            repaint();  // 重写绘制图像
            try {
                Thread.sleep(50); //线程休眠 50 毫秒

                // 判断马里奥是否在屏幕最右边，如果是切换场景
                if (mario.getX() >= 775){
                    NowBackground = AllBackground.get(NowBackground.getSort());
                    mario.setBackGround(NowBackground); // 将当前场景对象传递给马里奥类
                    mario.setX(10);   // 重置马里奥坐标，到出生点位置
                    mario.setY(395);
                }

                // 马里奥是否死亡
                if (mario.isDeath()){
                    JOptionPane.showMessageDialog(this,"马里奥死亡演示结束 - Arvin");
                    System.exit(0); // 结束程序
                }

                // 判断游戏是否结束
                if (mario.isOK()) { //判断是否结束 弹出一个窗口 调用JOptionPane 的 showMessageDialog 方法
                    JOptionPane.showMessageDialog(this,"Demo演示结束 - Arvin");
                    System.exit(0); // 结束程序
                }

            } catch (InterruptedException exception) {
                exception.printStackTrace();
            }
        }
    }
}
```