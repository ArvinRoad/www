---
title: Java 音乐类（完结）
date: 2021/11/15 16:27:52
tags:
  - 程序
categories:
  - CodeGame
cover: https://www.ign.com.cn/sm/ign_cn/screenshot/default/5_qw6f.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
```java
package com.arvin;

import javazoom.jl.decoder.JavaLayerException;
import javazoom.jl.player.Player;

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;

/**
 *  背景音乐类
 */

public class Music {

    // 空参构造
    public Music() throws FileNotFoundException, JavaLayerException {
        Player player;  // 播放音乐
        String str = System.getProperty("user.dir") + "/src/Music/music.wav";
        BufferedInputStream name = new BufferedInputStream(new FileInputStream(str)); // 读取 str 中音乐 将异常添加到方法签名
        player = new Player(name); // 实例化 Player 对象 将异常添加到方法签名
        player.play(); // 调用 Player 的 play() 开始播放音乐
    }

}
```