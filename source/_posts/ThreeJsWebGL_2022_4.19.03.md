---
title: Three.Js开发WebGL动画实现与请求动画帧
date: 2022/4/19 18:01:00
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
# Three.Js开发WebGL动画实现与请求动画帧

> Three.Js GitHub地址：[点击跳转](https://github.com/mrdoob/three.js)

> Three.Js API说明文档：[点击跳转](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)

> 或复制网页端Three.js源码使用：[点击跳转](https://threejs.org/build/three.js)


## WEBGL 动画实现与请求动画帧(放置在Script标签下)：
动画渲染函数(不推荐使用：如果性能不足情况下间隔函数有可能形成卡帧现象)
```javascript
function render(){
    renderer.render(scene,camera);
    cube.rotateY(0.01); // 每帧绕 Y 轴旋转 0.01 弧度
}
setInterval(render,16); // 间隔函数 16毫秒旋转一次 60帧1s
```
采用请求动画帧的方式可以确保我们的动画不会出现卡帧现象
```javascript
// 渲染函数
function render(){
    renderer.render(scene,camera);
    cube.rotateY(0.01);
    window.requestAnimationFrame(render);   // 递归
}
// 请求动画帧
window.requestAnimationFrame(render);
```
requestAnimationFrame 很好的一个特点，它会按照浏览器的性能合理的安排计算和渲染。

requestAnimationFrame 请求间隔时间是不定的，所以我们要配套的进行时间的计算。
```javascript
// 计算渲染时间
let T0 = new Date();

// 渲染函数
function render(){
    let T1 = new Date();
    let T = T1 - T0;    //  T 为时间差
    T0 = T1;
    renderer.render(scene,camera);
    cube.rotateY(0.001 * T); // 每1毫旋转0.001 * 时间差
    window.requestAnimationFrame(render);   // 递归
}
// 请求动画帧
window.requestAnimationFrame(render);
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
        // 设置渲染物体阴影
        renderer.shadowMapEnabled = true;
        // 创建显示卡迪坐标轴
        var axes = new THREE.AxesHelper(20);
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
        plane.position.x = 0;
        plane.position.y = 0;
        plane.position.z = 0;
        //- 设置地面接收阴影
        plane.castShadow = true;
        plane.receiveShadow = true;
        // 将地面添加到场景
        scene.add(plane);

        // 添加立方体
        var cubeGeometry = new THREE.BoxGeometry(4,4,4);
        var cubeMaterial = new THREE.MeshLambertMaterial({color: 0xFFB6C1}); // 添加网格材质
        var cube = new THREE.Mesh(cubeGeometry,cubeMaterial);
        cube.position.x = 0;
        cube.position.y = 5;
        cube.position.z = 2;
        // 球体
        var sphereGeometry = new THREE.SphereGeometry(4,20,20);
        var sphereMaterial = new THREE.MeshLambertMaterial({color: 0xFFB6C1});
        var Sphere = new THREE.Mesh(sphereGeometry,sphereMaterial);
        Sphere.position.x = 10;
        Sphere.position.y = 4;
        Sphere.position.z = 2;
        // 对象是否渲染到阴影贴图
        cube.castShadow = true;
        Sphere.castShadow = true;
        scene.add(cube)
        scene.add(Sphere);

        // 创建添加聚光灯
        var spotLight = new THREE.SpotLight(0xFFFFFF);
        spotLight.position.set(130,130,-130);
        spotLight.castShadow = true;
        scene.add(spotLight);

        // 定位摄影机视角指向场景中心点
        camera.position.x = 30;
        camera.position.y = 30;
        camera.position.z = 30;
        camera.lookAt(scene.position)
        // 渲染器输出添加至 HTML
        document.getElementById('webGl-output').appendChild(renderer.domElement);
        // 最终渲染
        renderer.render(scene,camera);

        // 计算渲染时间
        let T0 = new Date();

        // 渲染函数
        function render(){
            let T1 = new Date();
            let T = T1 - T0;    //  T 为时间差
            T0 = T1;
            renderer.render(scene,camera);
            cube.rotateY(0.001 * T); // 每1毫旋转0.001 * 时间差
            window.requestAnimationFrame(render);   // 递归
        }
        // 请求动画帧
        window.requestAnimationFrame(render);
    }
    window.onload = init    // 全部加载完成后运行
</script>
</body>
</html>
```