---
title: Docs with VitePress
editLink: true
---
{{ $frontmatter.title }}

# 速成Three.js——一、初识Three以及基础的前端场景搭建(结尾含源码)

文章：
1. [Three——一、初识Three以及基础的前端场景搭建(结尾含源码)](https://blog.csdn.net/nanchen_J/article/details/130109054?spm=1001.2014.3001.5501)
2. [Three——二、加强对三维空间的认识](https://blog.csdn.net/nanchen_J/article/details/130319637?spm=1001.2014.3001.5501)
3. [Three——三、动画执行、画布大小、渲染帧率和相机适配体验](https://blog.csdn.net/nanchen_J/article/details/130489240?spm=1001.2014.3001.5501)
4. [Three——四、几何体、高光网络材质、锯齿模糊以及GUI库的使用](https://blog.csdn.net/nanchen_J/article/details/130525430?spm=1001.2014.3001.5501)
5. [Three——五、点线模型对象、三角形概念、几何体顶点位置，顶点索引、法线以及对几何体进行旋转缩放和平移](https://blog.csdn.net/nanchen_J/article/details/130576232?spm=1001.2014.3001.5501)

给模型添加标签时需要准标签部分
从本章开始会从最初的搭建场景模型开始到插入精灵图部分结尾，便于刚入门three而不知如何去学起的前端工程师去学习，这里可以学到场景搭建的基础知识，引入外部模型以及动画，对模型添加标签以及优化模型方案等。详情可查找[Three.js教程](http://www.webgl3d.cn/)


搭建3D场景需要认识一下`场景Scene`、`相机Camera`、`渲染器Renderer`三个概念，看张图片
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/995511d99596549757e8e75b52aa9130_47a025488d7e46ce8edca2d4eb05117b-1.png)


在创建模型在页面中显示出来时我们需要先创建一个场景(这里所用到的东西都在THREE中，直接`new Three.(属性)`就可以了)  
```js
// 创建一个场景
let scene = new Three.Scene()
```

## 物体形状：几何体Geometry
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/d2de35b412d1a96ab0133eef330d4197_dd967c9f00314aa4828b1ab919e12e93.png)

例如：
```js
const geometry = new THREE.BoxGeometry(100, 100, 100); 
```

## 物体外观：材质Material
如果你想定义物体的外观效果，比如颜色，就需要通过材质Material相关的API实现。
threejs不同材质渲染效果不同，下面就以threejs最简单的网格基础材质MeshBasicMaterial (opens new window)为例给大家实现一个红色材质效果。
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/d2fc6d6da11471bb12fccba1459e848b_2957ecb5f95744478f41f16373cc762d.png)

```js
//创建一个材质对象Material
const material = new THREE.MeshBasicMaterial({
    color: 0xff0000,//0xff0000设置材质颜色为红色
}); 
```
## 物体：网格模型Mesh
这里实际将几何体和材质拼接在网络模型中，形成一个完整的几何体模型
```js
// 两个参数分别为几何体geometry、材质material
const mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
```


## 模型位置.position
```js
const mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
//设置网格模型在三维空间中的位置坐标，默认是坐标原点
mesh.position.set(0,10,0);
```

## .add()方法
在threejs中你创建了一个表示物体的虚拟对象Mesh，需要通过.add()方法，把网格模型mesh添加到三维场景scene中。
```js
scene.add(mesh); 
```

下面两个方法将是执行渲染效果的核心部分，`相机Camera`和`渲染器Renderer`

## 透视投影相机PerspectiveCamera
Threejs提供了正投影相机OrthographicCamera (opens new window)和透视投影相机PerspectiveCamera (opens new window)。本节课先给大家比较常用的透视投影相机PerspectiveCamera。

<p style='color:red'>透视投影相机PerspectiveCamera本质上就是在模拟人眼观察这个世界的规律。</p>

```js
// 实例化一个透视投影相机对象
const camera = new THREE.PerspectiveCamera();
```


## 相机位置.position
生活中用相机拍照，你相机位置不同，拍照结果也不同，threejs中虚拟相机同样如此。

比如有一间房子，你拿着相机站在房间里面，看到的是房间内部，站在房子外面看到的是房子外面效果。

相机对象Camera具有位置属性.position，通过位置属性.position可以设置相机的位置。
```js
//相机在Three.js三维坐标系中的位置
// 根据需要设置相机位置具体值
camera.position.set(200, 200, 200); 
```

## 相机观察目标.lookAt()
你用相机拍照你需要控制相机的拍照目标，具体说相机镜头对准哪个物体或说哪个坐标。对于threejs相机而言，就是设置.lookAt()方法的参数，指定一个3D坐标。
```js
//相机观察目标指向Threejs 3D空间中某个位置
camera.lookAt(0, 0, 0); //坐标原点
camera.lookAt(0, 10, 0);  //y轴上位置10
camera.lookAt(mesh.position);//指向mesh对应的位置
```
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/613410420e365d9e1383a0311598a4c0_2b29a989f0f647c2a067143f2b588a84.png)


## 判断相机相对三维场景中长方体位置
你可以把三维场景中长方体`mesh`想象为一个房间，然后根据相机位置和长方体位置尺寸对比，判断两者相对位置。你可以发现设置相机坐标(200, 200, 200)，位于长方体外面一处位置。

```js
// 长方体尺寸100, 100, 100
const geometry = new THREE.BoxGeometry( 100, 100, 100 );
const mesh = new THREE.Mesh(geometry,material);
// 相机位置xyz坐标：0,10,0
mesh.position.set(0,10,0);
// 相机位置xyz坐标：200, 200, 200
camera.position.set(200, 200, 200); 
```
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/bccee1a50ca25571a83b0bc8b1564fac_3df3353110bb432aa2603cbf101fdcb2.png)

说简单一些就是在一个三维场景中，有一个网络模型的坐标，还有一个是相机的坐标

## *透视投影相机PerspectiveCamera：视锥体
透视投影相机的四个参数fov, aspect, near, far构成一个四棱台3D空间，被称为视锥体，只有视锥体之内的物体，才会渲染出来，视锥体范围之外的物体不会显示在Canvas画布上。下面这张图解释的已经很清晰了，主要的意思就是这个属性会把相机视为一个锥体，如果你放大缩小的话也只会显示锥体里面的模型样子
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/5bda1f451ac63e2a34ab14ab778e8574_4afc18ebd63c436492ae44b7b738e24f.png)

```js
// width和height用来设置Three.js输出的Canvas画布尺寸(像素px)
const width = 800; //宽度
const height = 500; //高度
// 30:视场角度, width / height:Canvas画布宽高比, 1:近裁截面, 3000：远裁截面
const camera = new THREE.PerspectiveCamera(30, width / height, 1, 3000);
```
参数介绍：
```js
PerspectiveCamera( fov, aspect, near, far )
```
| 参数 | 含义                                                                       | 推荐默认值 |
| ------ | ---------------------------------------------------------------------------- | ------ |
| fov    | 视场：即可以看到的角度范围，人的视场大约是180度，一般游戏的视场为60度到90度                        | 60     |
| aspect | 长宽比：这个是渲染结果输出区域的横向长度和纵向长度的比值；长宽比决定了水平视场和垂直视场之间的比例关系，推荐默认值：window.innerWidth/window.innerHeight      |window.innerWidth/window.innerHeight
| near   | 近面：基于相机位置，表示从这里开始渲染场景；一般会设置一个很小的值                               | 0.1    |
| far    | 远面：基于相机位置，表示停止渲染的位置；要注意设置合适的距离，如果设置太小，部分场景可能渲染不到，但如果设置的太大，会影响渲染的效率 |  1000      |




# 渲染器
生活中如果有了景物和相机，那么如果想获得一张照片，就需要你拿着相机，按一下，咔，完成拍照。对于threejs而言，如果完成“咔”这个拍照动作，就需要一个新的对象，也就是WebGL渲染器`WebGLRenderer`
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/e248010dc1cc534a4faf94302c0f222f_bf9d5da6ee0f428b9e5eabd437dba260.png)
所以想要生成一张渲染在浏览器中的`照片`，就需要模型，相机以及渲染器

## WebGL渲染器WebGLRenderer
通过WebGL渲染器WebGLRenderer (opens new window)可以实例化一个WebGL渲染器对象。
```js
// 创建渲染器对象
const renderer = new THREE.WebGLRenderer();
```
## 设置Canvas画布尺寸.setSize()

```js
// 定义threejs输出画布的尺寸(单位:像素px)
const width = 800; //宽度
const height = 500; //高度
renderer.setSize(width, height); //设置three.js渲染区域的尺寸(像素px)
```
通常简单来说就是渲染多大的地方，我这里的话就按浏览器全屏来算了
，参数的话可以给`clientWidth`,`clientHeight`

## 渲染器渲染方法`.render()`
渲染器WebGLRenderer执行渲染方法`.render()`就可以生成一个Canvas画布(照片)，并把三维场景Scene呈现在canvas画布上面,你可以把`.render()`理解为相机的拍照动作“咔”。这里也很好理解，就是`.render()`已经生成好了画布，但还没渲染在浏览器中
```js
renderer.render(scene, camera); //执行渲染操作
```

## 渲染器Canvas画布属性.domElement
渲染器WebGLRenderer通过属性.domElement可以获得渲染方法.render()生成的Canvas画布，.domElement本质上就是一个HTML元素：Canvas画布。到这里就可以把生成的这个画布`canvas`放在指定的标签中在浏览器里显示出来
```js
document.body.appendChild(renderer.domElement);
```
## Canvas画布插入到任意HTML元素中
```js
document.getElementById('webgl').appendChild(renderer.domElement);
```


接下来我们理解了基础用法，来用V3实现一个简单的demo
```html
<!-- author: Mr.J -->
<!-- date: 2023-04-12 11:43:45 -->
<!-- description: Vue3+JS代码块模板 -->
<template>
  <div class="container" ref="container">
  </div>
</template>

<script setup>
import * as THREE from "three";
// 轨道
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { ref, reactive, onMounted } from "vue";
// 三个必备的参数
let scene, camera, renderer, controls;

onMounted(() => {
  // 外层需要获取到dom元素以及浏览器宽高，来对画布设置长宽
  // clientWidth等同于container.value.clientWidth
  let container = document.querySelector(".container");
  const { clientWidth, clientHeight } = container;
  console.log(clientHeight);

  init();
  animate();
  // 首先需要获取场景，这里公共方法放在init函数中
  function init() {
    scene = new THREE.Scene();
    // 给相机设置一个背景
    scene.background = new THREE.Color(0x000000);
    // 透视投影相机PerspectiveCamera
    // 支持的参数：fov, aspect, near, far
    camera = new THREE.PerspectiveCamera(
      60,
      clientWidth / clientHeight,
      0.1,
      1000
    );
    // 相机坐标
    camera.position.set(200, 200, 200);
    // 相机观察目标
    camera.lookAt(scene.position);
    // 渲染器
    renderer = new THREE.WebGLRenderer();
    // 渲染多大的地方
    renderer.setSize(clientWidth, clientHeight);
    container.appendChild(renderer.domElement);
    controls = new OrbitControls(camera, renderer.domElement);
    addBox();
  }

  function addBox() {
    // 模型部分
    // 几何体
    const geometry = new THREE.BoxGeometry(100, 100, 100);
    // 材质
    const material = new THREE.MeshBasicMaterial({
      color: 0x51efe4, //0x51efe4设置材质颜色
    });
    // 网络模型
    const mesh = new THREE.Mesh(geometry, material);
    mesh.position.set(0, 10, 0);
    scene.add(mesh);
  }
  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
});
</script>

<style>
.container {
  width: 100%;
  height: 100vh;
  position: relative;
  z-index: 1;
}
</style>

```
效果展示:
![Alt text](assets/three01%E5%9C%BA%E6%99%AF%E6%90%AD%E5%BB%BA/03f88bcae1b48b1b443b8e6ae1861392_6f896519a7e04deb8499d0c18d60e9ec.gif)
