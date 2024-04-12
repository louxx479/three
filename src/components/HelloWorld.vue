<script setup>
import { ref } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import {GLTFLoader }from 'three/examples/jsm/loaders/GLTFLoader.js';
    const loader = new GLTFLoader()

defineProps({
  msg: String,
})
    const scene = new THREE.Scene(); 
    //var geometry = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
    //var geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
   // var box = new THREE.SphereGeometry(60,40,40);//创建一个球的模型 （半径，精度）
    //var box1 =  new THREE.CylinderGeometry(50,50,100,25);//创建一个圆柱（顶部，底部，高度，精度）
   // var geometry1 = new THREE.BufferGeometry(); //创建一个Buffer类型几何体对象
    // var vertices =new Float32Array([
    //   0, 0, 0,
    //   0, 10, 0,
    //   0, 10, 10,
    //   0, 10, 0,
    //   0, 10, 10,
    //   20, 10, 0,
    //   0, 10, 10,
    //   20, 10, 0,
    //   20, 10, 10,
    //   20, 0, 0,
    //   20, 10, 0,
    //   20, 10, 10,
    // ])
    // 创建属性缓冲区对象
    //var attribue = new THREE.BufferAttribute(vertices, 3); //3个为一组，表示一个顶点的xyz坐标
    // 设置几何体attributes属性的位置属性
    //geometry1.attributes.position = attribue;
    // 三角面(网格)渲染模式
    // var material = new THREE.MeshBasicMaterial({
    //   color: 0x0000ff, //三角面颜色
    //   side: THREE.DoubleSide //两面可见
    // }); //材质对象
    //var mesh2 = new THREE.Mesh(geometry1, material);
    // MeshBasicMaterial	基础网格材质，不受光照影响的材质
    // MeshLambertMaterial	Lambert网格材质，与光照有反应，漫反射
    // MeshPhongMaterial	高光Phong材质,与光照有反应
    // MeshStandardMaterial	PBR物理材质，相比较高光Phong材质可以更好的模拟金属、玻璃等效果
    var material = new THREE.MeshLambertMaterial({
      color: 0x64B5D6,
      //wireframe:true,
      opacity:1,//透明度设置，0表示完全透明，1表示完全不透明
      transparent:false,//是否开启透明，默认false
    }); //材质对象Material
    //var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    //var mesh1 = new THREE.Mesh(box, material); //网格模型对象Mesh
    //var mesh2 = new THREE.Mesh(box1, material); //网格模型对象Mesh
    // mesh.castShadow=true;
    // mesh.receiveShadow=true;
    // scene.add(mesh); //网格模型添加到场景中
   // mesh1.translateY(120);
    //scene.add(mesh1); //网格模型添加到场景中
    //mesh2.position.set(120,0,0);//设置mesh3模型对象的xyz坐标为120,0,0
    //scene.add(mesh2); //网格模型添加到场景中
    loader.load('./assets/bend.glb',
      gltf => {
        scene.add(gltf.scene) // 添加到场景中
      } 
    );
    

    /**
     * 光源设置
     */

    //环境光
    var ambient = new THREE.AmbientLight(0xffffff,10);
    ambient.position.set(400,400,400)
    scene.add(ambient);
    //点光源
    var point = new THREE.PointLight(0xffc0cb,10, 0, 0);
    point.position.set(400, 300, 200); //点光源位置
    point.castShadow = true;
    scene.add(point); //点光源添加到场景中
    // var point2 = new THREE.PointLight(0xffffff);
    // point2.position.set(-400, -200, -300); //点光源位置
    // scene.add(point2); //点光源添加到场景中
    /**
     * 相机设置
     */
    var width = 900; //窗口宽度
    var height = 900; //窗口高度
    var k = width / height; //窗口宽高比
    var s = 1000; //三维场景显示范围控制系数，系数越大，显示的范围越大
    //创建相机对象
    var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
    camera.position.set(100, 100, 100); //设置相机位置
    camera.lookAt(scene.position); //设置相机方向(指向的场景对象)
    /**
     * 创建渲染器对象
     */
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, height);//设置渲染区域尺寸
    renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
    document.body.appendChild(renderer.domElement); //body元素中插入canvas对象
    //执行渲染操作   指定场景、相机作为参数
    renderer.render(scene, camera);

    let T0 = new Date();//上次时间
    function render() {
        //let T1 = new Date();//本次时间
        //let t = T1-T0;//时间差
        //T0 = T1;//把本次时间赋值给上次时间
        //requestAnimationFrame(render);
       // renderer.render(scene,camera);//执行渲染操作
        //mesh.rotateY(0.001*t);//旋转角速度0.001弧度每毫秒
        renderer.render(scene, camera);
        //requestAnimationFrame(render);
    }
    render();
    var controls = new OrbitControls(camera, renderer.domElement);

    controls.addEventListener('change', render);//监听鼠标、键盘事件
</script>

<template>
  
  <h1>{{ msg }}</h1>


</template>

<style scoped>
    body {
      margin: 0;
      overflow: hidden;
      /* 隐藏body窗口区域滚动条 */
    }

</style>
