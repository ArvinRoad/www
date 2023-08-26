---
title: Java 马里奥类
date: 2021/11/12 19:28:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://tse3-mm.cn.bing.net/th/id/OIP-C.yhipSAzNiVulV4MZu2cVJAHaEH?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
#### 代码演示

```java
package com.arvin;

import java.awt.image.BufferedImage;

/**
 *  马里奥类
 */

// 利用马里奥类实现 Runnadle 接口 重写其中的抽象方法
public class Mario implements Runnable {

    // 用于表示(存储)横纵坐标
    private int x, y;

    // 用于表示马里奥当前状态
    private String status;

    // 显示当前状态对应的图像
    private BufferedImage show = null;

    // 定义一个 BackGround 对象用来获取障碍物信息
    private BackGround backGround = new BackGround();

    // 用来实现马里奥动作 线程对象
    private Thread thread = null;

    // 马里奥的移动速度
    private int xSpeed;

    // 马里奥的跳跃速度
    private int ySpeed;

    // 定义一个索引 取得马里奥的运动图像
    private int indexOf;

    // 表示马里奥上升的时间
    private int upTime = 0;

    // 判断马里奥是否走到了城堡门口
    private boolean isOK;

    // 判断马里奥是否死亡
    private boolean isDeath = false;

    // 积分系统 表示分数
    private int score = 0;

    // 定义构造函数
    public Mario(){

    }

    public Mario(int x, int y){
        this.x = x;
        this.y = y;
        show = StaticValue.Stand_Right;     // 初始化 show
        this.status = "stand--Right";     // 用于表示马里奥当前的状态
        thread = new Thread(this);    // 初始化线程
        thread.start();                     // 启动线程 调用 start 方法

    }

    // 马里奥死亡方法
    public void death() {
        isDeath = true;
    }

    // 马里奥向左移动
    public void leftMove () {

        // 改变移动速度
        xSpeed = -5;

        // 判断马里奥是否碰到了旗子 马里奥就应该无法移动
        if (backGround.isReach()) {
            xSpeed = 0;
        }

        // 判断马里奥是否处于空中
        if (status.indexOf("jump") != -1){
            status = "jump--left";
        }else{
            status = "move--left";
        }
    }

    // 马里奥向右移动
    public void rightMove () {
        xSpeed = 5;

        // 判断马里奥是否碰到了旗子 马里奥就应该无法移动
        if (backGround.isReach()) {
            xSpeed = 0;
        }

        if (status.indexOf("jump") != -1){
            status = "jump--right";
        }else{
            status = "move--right";
        }
    }

    // 马里奥向左停止
    public void leftStop () {
        xSpeed = 0;
        if (status.indexOf("jump") != -1){
            status = "jump--left";
        }else{
            status = "stop--left";
        }
    }

    // 马里奥向右停止
    public void rightStop () {
        xSpeed = 0;
        if (status.indexOf("jump") != -1){
            status = "jump--right";
        }else{
            status = "stop--right";
        }
    }

    // 马里奥跳跃
    public void jump () {
        if (status.indexOf("jump") == -1){  // 判断马里奥是否是跳跃状态(-1 表示不是)
            if (status.indexOf("left") != -1) {  // 判断马里奥此时的方向(-1 表示向左)
                status = "jump--left";
            }else{
                status = "jump--right";
            }
            ySpeed = -10;
            upTime = 7;
        }

        // 判断马里奥是否碰到了旗子 马里奥就应该无法移动
        if (backGround.isReach()) {
            ySpeed = 0;
        }
    }

    // 马里奥下落
    public void fall () {
        if (status.indexOf("left") != -1){
            status = "jump--left";
        }else{
            status = "jump--right";
        }
        ySpeed = 10;
    }

    // 重写 Runnable 的抽象方法
    @Override
    public void run() {

        // while 死循环
        while (true) {

            // 判断马里奥是否处于障碍物上
            boolean onObstacle = false;

            // 判断是否可以往右走
            boolean canRight = true;

            // 判断是否可以往左走
            boolean canLeft = true;

            // 判断马里奥是否到达旗杆位置 (游戏结束)
            if (backGround.isFlag() && this.x >= 500){
                this.backGround.setReach(true);
                if(this.backGround.isBase()) { // 判断旗子是否下落完成
                    status = "move--right";
                    if (x < 690) { // 判断马里奥是否移动到了中间
                        x += 5;
                    }else{
                        isOK = true;
                    }
                }else{
                    if (y < 395) {
                        xSpeed = 0;
                        this.y += 5;
                        status = "jump--right";
                    }

                    if (y > 395) {
                        this.y = 395;
                        status = "stop--right";
                    }
                }

            }else{

                // 遍历当前场景里所有的障碍物
                for (int i = 0; i <backGround.getObstacleList().size(); i++) {
                    Obstacle obstacle = backGround.getObstacleList().get(i); // 临时变量存储当前场景障碍物信息
                    if (obstacle.getY() == this.y + 25 && (obstacle.getX() > this.x -30 && obstacle.getX() < this.x +25)){ // 判断马里奥是否处于障碍物上
                        onObstacle = true;
                    }

                    // 判断是否跳起来顶到砖块 (碰撞检测)
                    if ((obstacle.getY() >= this.y -30 && obstacle.getY() <= this.y -20) && (obstacle.getX() > this.x -30 && obstacle.getX() < this.x + 25)){
                        if (obstacle.getType() == 0) { // 检测障碍物类型 (0 可破坏砖块)
                            backGround.getObstacleList().remove(obstacle);
                            score += 1; // 积分+1
                        }
                        upTime = 0; // 行为反馈 顶到砖块立刻下落
                    }

                    // 判断是否可以往右走 (碰撞检测)
                    if (obstacle.getX() == this.x + 25 && (obstacle.getY() > this.y -30 && obstacle.getY() < this.y +25)){
                        canRight = false;
                    }

                    // 判断是否可以往左走 (碰撞检测)
                    if (obstacle.getX() == this.x - 30 && (obstacle.getY() > this.y - 30 && obstacle.getY() < this.y + 25)) {
                        canLeft = false;
                    }
                }

                // 判断马里奥是否碰到敌人死亡或者踩死敌人
                for (int i = 0; i < backGround.getEnemyList().size(); i++) { // 遍历每一个敌人
                    Enemy enemy = backGround.getEnemyList().get(i); // enemy 存储当前的敌人

                    // 判断马里奥是否位于敌人头上 与判断障碍物是一样的
                    if (enemy.getY() == this.y + 20 && (enemy.getX() -25 <= this.x && enemy.getX() +35 >= this.x)) {
                        // 判断是蘑菇敌人还是食人花敌人 蘑菇敌人才可以踩死
                        if (enemy.getType() == 1) {
                            enemy.death();  // 调用蘑菇敌人的死亡方法
                            score += 2;

                            // 让马里奥上升一小段
                            upTime = 3;
                            ySpeed = -10;
                        }else if (enemy.getType()== 2){ // 食人花敌人情况
                            // 马里奥死亡
                            death();
                        }
                    }
                    // 判断马里奥是否碰到敌人 碰到死亡
                    if ((enemy.getX() + 35 > this.x && enemy.getX() - 25 < this.x) && (enemy.getY() + 35 >this.y && enemy.getY() -20 < this.y)) {
                        // 马里奥死亡
                        death();
                    }
                }

                // 进行马里奥跳跃
                if (onObstacle && upTime == 0){
                    if (status.indexOf("left") != -1){
                        if (xSpeed != 0){
                            status = "move--left";
                        }else{
                            status = "stop--left";
                        }
                    }else{
                        if (xSpeed != 0){
                            status = "move--right";
                        }else{
                            status = "stop--right";
                        }
                    }
                }else{
                    if (upTime != 0){
                        upTime--;   // 一直自检
                    }else{
                        fall(); // 让他下落
                    }
                    y += ySpeed; // 改变坐标值
                }


                // 判断马里奥是否在运动
                if (canLeft && xSpeed < 0 || canRight && xSpeed > 0) {
                    x += xSpeed;

                    // 判断马里奥是否到了最左边
                    if (x < 0){
                        x = 0;
                    }
                }

                // 判断当前是否是移动状态
                if (status.contains("move")) {
                    indexOf = indexOf == 0 ? 1 : 0;
                }

                // 判断是否向左移动
                if ("move--left".equals(status)) {
                    show = StaticValue.Run_Left.get(indexOf);
                }

                // 判断是否向右移动
                if ("move--right".equals(status)) {
                    show = StaticValue.Run_Right.get(indexOf);
                }

                // 判断是否向左停止
                if ("stop--left".equals(status)) {
                    show = StaticValue.Stand_Left;
                }

                // 判断是否向右停止
                if ("stop-right".equals(status)) {
                    show = StaticValue.Stand_Right;
                }

                // 判断是否向左跳跃
                if ("jump--left".equals(status)){
                    show = StaticValue.Jump_Left;
                }

                // 判断是否向右跳跃
                if ("jump--right".equals(status)){
                    show = StaticValue.Jump_Right;
                }

                // 线程休眠 50 毫秒
                try {
                    Thread.sleep(50);
                } catch (InterruptedException exception) {
                    exception.printStackTrace();
                }

            }
        }
    }


    // 生成 x y BufferedImage 的 set 和 get 方法
    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }

    public BufferedImage getShow() {
        return show;
    }

    public void setShow(BufferedImage show) {
        this.show = show;
    }

    // 生成 backGround 的 set 方法
    public void setBackGround(BackGround backGround) {
        this.backGround = backGround;
    }

    // 生成 判断马里奥是否到城堡门口的 Get 方法
    public boolean isOK() {
        return isOK;
    }

    // 生成 isDeath 的 Get 方法
    public boolean isDeath() {
        return isDeath;
    }

    // 生成分数的 Get 方法
    public int getScore() {
        return score;
    }
}

```