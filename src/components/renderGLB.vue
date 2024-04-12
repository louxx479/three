<script setup>
import { ref } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import Stats from 'three/examples/jsm/libs/stats.module';
    const loader = new GLTFLoader()
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);

    //创建性能监控器
    const stats = new Stats;
    document.body.appendChild(stats.domElement);
    // 设置相机的位置
    camera.position.x = 100; // 相机在x轴上的位置
    camera.position.y = 100; // 相机在y轴上的位置
    camera.position.z = 200; // 相机在z轴上的位置
    const renderer = new THREE.WebGLRenderer({
        antialias: true
    });
    renderer.setSize(500, 500);
    renderer.setPixelRatio(window.devicePixelRati || 1);
    document.body.appendChild(renderer.domElement);
    //创建辅助坐标系
    const axesHelper = new THREE.AxesHelper(100);
    scene.add(axesHelper);
    //创建辅助平面
    const GridHelper = new THREE.GridHelper(100);
    scene.add(GridHelper);

    //环境光
    // 创建环境光
    const ambientLight = new THREE.AmbientLight(0xffffff, 1); // 纯白色光，强度为0.5
    scene.add(ambientLight);
    var point = new THREE.PointLight(0xFFD700, 5, 0, 0);
    point.position.set(400, 300, 200); //点光源位置
    point.castShadow = true;
    scene.add(point); //点光源添加到场景中
    
    loader.load('src/assets/rat.glb', function (gltf) {

        // 假设gltf是一个加载完成的glTF模型对象
        gltf.scene.traverse(child => {
        if (child.isMesh) {
            child.material = new THREE.MeshStandardMaterial({ 
                color: 0xffffff * Math.random(),
            });
            //child.position.set(0, 0, -0); // 设置x, y, z坐标
             // 重置模型位置和缩放
            gltf.scene.position.set(0, 0, 0);
            gltf.scene.scale.set(1, 1, 1);
        }
        });
        scene.add(gltf.scene);
    }, undefined, function (error) {
        console.error(error);
    });
    //创建轨道控制器
   
    function render() {
        requestAnimationFrame(render);
        renderer.render(scene, camera);
    }
    render();

    var controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    const clock = new THREE.Clock();
    console.log(scene)
    const tick =()=>{
        const elapsedTime = clock.getElapsedTime();

        camera.position.x = Math.cos(elapsedTime)*300;
        camera.position.y = Math.sin(elapsedTime)*300;
        //console.log(elapsedTime)
        //scene.position.x += Math.sin(elapsedTime);
        //scene.position.y += Math.cos(elapsedTime);
        stats.update();
        controls.update();
        renderer.render(scene, camera);
        window.requestAnimationFrame(tick);
    }
    tick();

</script>

<template>
  
</template>

<style scoped>
    body {
      margin: 0;
      overflow: hidden;
      /* 隐藏body窗口区域滚动条 */
    }

</style>
