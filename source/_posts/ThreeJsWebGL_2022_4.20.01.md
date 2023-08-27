---
title: Three.Js开发WebGL地月环绕
date: 2022/4/21 15:19:00
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
# Three.Js开发WebGL地月环绕

> Three.Js GitHub地址：[点击跳转](https://github.com/mrdoob/three.js)

> Three.Js API说明文档：[点击跳转](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)

> 或复制网页端Three.js源码使用：[点击跳转](https://threejs.org/build/three.js)

> 演示网站：[点击跳转](https://arvinroad.github.io/3DEAMS/)


## 完整实例代码：
```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>3D地月环绕</title>
    <style type="text/css">
        *{
            margin: 0;
            padding: 0;
        }
        /* 背景 */
        canvas{
            background-image: url(resource/Demo04/imgs/starts.jpg);
            background-size: cover;
        }
        .label{
            color: #FFF;
            font-size: 16px;
        }
    </style>
    <script type="module">
        import *as THREE from '/libs/build/three.module.js';
        import { OrbitControls } from '/libs/jsm/controls/OrbitControls.js';
        import { CSS2DRenderer,CSS2DObject } from '/libs/jsm/renderers/CSS2DRenderer.js';

        // 声明全局变量
        let camera,scene,renderer,labelRenderer;
        let moon,earth;
        let clock = new THREE.Clock();
        const textureLoader = new THREE.TextureLoader();    // 实例化纹理加载器

        function init(){
            // 地球月球半径大小
            const EARTH_RADIUS = 2.5;
            const MOON_RADIUS = 0.27;
            // 透视相机
            camera = new THREE.PerspectiveCamera(45,window.innerWidth/window.innerHeight,0.1,200);
            camera.position.set(10,5,20);
            // 实例化场景
            scene = new THREE.Scene();
            // 聚光灯
            const dirLight = new THREE.SpotLight(0xFFFFFF);
            dirLight.position.set(0,0,10);
            dirLight.intensity = 2; // 光源强度
            dirLight.castShadow = true; // 光照阴影
            scene.add(dirLight);
            // 添加环境光
            const aLight = new THREE.AmbientLight(0xFFFFFF);
            aLight.intensity = 0.3; // 调整环境光亮度
            scene.add(aLight);

            // 月球
            const moonGeometry = new THREE.SphereGeometry(MOON_RADIUS,16,16);
            const moonMaterial = new THREE.MeshPhongMaterial({ map:textureLoader.load('resource/Demo04/textures/planets/moon_1024.jpg') }); // 可高光材质
            moon = new THREE.Mesh(moonGeometry,moonMaterial);
            moon.receiveShadow = true; // 接收阴影
            moon.castShadow = true;
            scene.add(moon);
            // 地球
            const earthGeometry = new THREE.SphereGeometry(EARTH_RADIUS,16,16);
            const earthMaterial = new THREE.MeshPhongMaterial({
                shininess:5,    // 调低镜面反射
                map:textureLoader.load('resource/Demo04/textures/planets/earth_atmos_2048.jpg'),
                specularMap:textureLoader.load('resource/Demo04/textures/planets/earth_specular_2048.jpg'),
                normalMap:textureLoader.load('resource/Demo04/textures/planets/earth_normal_2048.jpg')
            })
            earth = new THREE.Mesh(earthGeometry,earthMaterial);
            earth.receiveShadow = true; // 接收阴影
            earth.castShadow = true;
            scene.add(earth);

            // 地球标签
            const earthDiv = document.createElement('div');
            earthDiv.className = 'label';
            earthDiv.textContent = '地球';
            const eartchLabel = new CSS2DObject(earthDiv);
            eartchLabel.position.set(0,EARTH_RADIUS+0.5,0);
            earth.add(eartchLabel);
            // 月球标签
            const MoonDiv = document.createElement('div');
            MoonDiv.className = 'label';
            MoonDiv.textContent = '月球';
            const MoonLabel = new CSS2DObject(MoonDiv);
            MoonLabel.position.set(0,MOON_RADIUS+0.5,0);
            moon.add(MoonLabel);

            // 渲染器
            renderer = new THREE.WebGLRenderer({
                alpha:true  // 透明度
            });
            renderer.setPixelRatio(window.devicePixelRatio);   // 像素比
            renderer.setSize(window.innerWidth,window.innerHeight);
            renderer.shadowMap.enabled = true;   // 渲染阴影
            document.body.appendChild(renderer.domElement); // 渲染到页面

            // 标签渲染器
            labelRenderer = new CSS2DRenderer();
            labelRenderer.setSize(window.innerWidth,window.innerHeight);
            labelRenderer.domElement.style.position = 'absolute';
            labelRenderer.domElement.style.top = '0px';
            document.body.appendChild(labelRenderer.domElement);

            // 绑定三维场景控制器
            const controls = new OrbitControls(camera,labelRenderer.domElement);
            controls.addEventListener('change',()=>{
                renderer.render(scene,camera);
                labelRenderer.render(scene,camera);
            })
        }
        var oldTime = 0;
        // 渲染动画函数
        function animate(){
            const elapsed = clock.getElapsedTime(); // 获取时间
            moon.position.set(Math.sin(elapsed)*5,0,Math.cos(elapsed)*5);    // (时间*半径)
            // 地球自转
            let axis = new THREE.Vector3(0,1,0); // 环绕 Y 轴
            earth.rotateOnAxis(axis,(elapsed - oldTime) * Math.PI / 10);  // 1s 转一下
            renderer.render(scene,camera);
            labelRenderer.render(scene,camera);
            oldTime = elapsed;
            requestAnimationFrame(animate);
        }

        init()
        animate()

        // 自动调整摄像机
        window.onreset = function (){
            camera.aspect = window.innerWidth/window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth,window.innerHeight);
        }
    </script>
</head>

<body>

</body>

</html>
```