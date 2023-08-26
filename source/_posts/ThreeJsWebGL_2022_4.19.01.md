---
title: Three.Js开发WebGL创建三维空间
date: 2022/4/19 14:46:00
tags:
  - 博客网站
  - 教学文档
categories:
  - 程序
cover: https://pic3.zhimg.com/v2-7af105b5915de8a7fdcf6332b681aba5_r.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
# Three.Js开发WebGL创建三维空间
首先我们需要创建一个 HTML 文件 如：index.html

导入Three.Js 与 设置网页初始化 (边距为0 溢出隐藏)
```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>Demo-01</title>
    <script src="libs/Three.js"></script>
    <style>
        body{
            margin: 0;
            overflow: hidden;
        }
    </style>
</head>

<body></body>
</html>
```
> Three.Js GitHub地址：[点击跳转](https://github.com/mrdoob/three.js)

> Three.Js API说明文档：[点击跳转](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)

> 或复制网页端Three.js源码使用：[点击跳转](https://threejs.org/build/three.js)

在 HTML文件中我们要创建容器
```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>Demo-01</title>
    <script src="libs/Three.js"></script>
    <style>
        body{
            margin: 0;
            overflow: hidden;
        }
    </style>
</head>

<body>

<div id="webGl-output">

</div>

<!-- Script -->
<script></script>
</body>
</html>
```
## WEBGL 三维画面效果的创建(放置在Script标签下)：
```javascript
    function init(){
        // 创建场景
        var scene = new THREE.Scene();
        // 设置摄像机 (45度,长宽比<当前屏幕宽/屏幕高>,最近可视距离,最远可视距离)
        var camera = new THREE.PerspectiveCamera(45,window.innerWidth/window.innerHeight,0.1,2000);
        // 创建渲染器
        var renderer = new THREE.WebGLRenderer();
        // 渲染器初始化颜色
        renderer.setClearColor(new THREE.Color(0xEEEEEE));
        // 渲染输出 Canvas 画面大小
        renderer.setSize(window.innerWidth,window.innerHeight);
        // 创建显示卡迪坐标轴
        var axes = new THREE.AxisHelper(20);
        // 添加坐标系到场景
        scene.add(axes);
        // 场景创建平面(地面几何体)
        var planeGeometry = new THREE.PlaneGeometry(60,20);
        // 设置平面(地面)材质
        var PlaneMaterial = new THREE.MeshBasicMaterial({color:0xCCCCCC});
        // 创建地面 通过 THREE.Mesh 进行结合操作
        var plane = new THREE.Mesh(planeGeometry,PlaneMaterial);
        // 物体位移坐标
        plane.rotation.x = -0.5*Math.PI;
        plane.position.x = 15;
        plane.position.y = 0;
        plane.position.z = 0;
        // 将地面添加到场景
        scene.add(plane);
        // 定位摄影机视角指向场景中心点
        camera.position.x = -30;
        camera.position.y = 40;
        camera.position.z = 30;
        camera.lookAt(scene.position)
        // 渲染器输出添加至 HTML
        document.getElementById('webGl-output').appendChild(renderer.domElement);
        // 最终渲染
        renderer.render(scene,camera);
    }
    window.onload = init    // 全部加载完成后运行
```
## 完整实例代码：
```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>Demo-01</title>
    <script src="libs/Three.js"></script>
    <style>
        body{
            margin: 0;
            overflow: hidden;
        }
    </style>
</head>

<body>

<div id="webGl-output">

</div>

<!-- Script -->
<script>
    function init(){
        // 创建场景
        var scene = new THREE.Scene();
        // 设置摄像机 (45度,长宽比<当前屏幕宽/屏幕高>,最近可视距离,最远可视距离)
        var camera = new THREE.PerspectiveCamera(45,window.innerWidth/window.innerHeight,0.1,2000);
        // 创建渲染器
        var renderer = new THREE.WebGLRenderer();
        // 渲染器初始化颜色
        renderer.setClearColor(new THREE.Color(0xEEEEEE));
        // 渲染输出 Canvas 画面大小
        renderer.setSize(window.innerWidth,window.innerHeight);
        // 创建显示卡迪坐标轴
        var axes = new THREE.AxisHelper(20);
        // 添加坐标系到场景
        scene.add(axes);
        // 场景创建平面(地面几何体)
        var planeGeometry = new THREE.PlaneGeometry(60,20);
        // 设置平面(地面)材质
        var PlaneMaterial = new THREE.MeshBasicMaterial({color:0xCCCCCC});
        // 创建地面 通过 THREE.Mesh 进行结合操作
        var plane = new THREE.Mesh(planeGeometry,PlaneMaterial);
        // 物体位移坐标
        plane.rotation.x = -0.5*Math.PI;
        plane.position.x = 15;
        plane.position.y = 0;
        plane.position.z = 0;
        // 将地面添加到场景
        scene.add(plane);
        // 定位摄影机视角指向场景中心点
        camera.position.x = -30;
        camera.position.y = 40;
        camera.position.z = 30;
        camera.lookAt(scene.position)
        // 渲染器输出添加至 HTML
        document.getElementById('webGl-output').appendChild(renderer.domElement);
        // 最终渲染
        renderer.render(scene,camera);
    }
    window.onload = init    // 全部加载完成后运行
</script>
</body>
</html>
```