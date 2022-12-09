

<template>
  <div class="content">
        <div class="domCanvas" ref="domCanvas"></div>
  </div>
</template>
<script setup>
import * as THREE from "three";
import { OrbitControls }  from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader }  from "three/examples/jsm/loaders/GLTFLoader";
import { DRACOLoader }  from "three/examples/jsm/loaders/DRACOLoader"; 
import gsap from "gsap";
// import * as dat from 'dat.gui'

import { onMounted, ref } from "vue";

// const gui = new dat.GUI({closed:true,width:500})
const domCanvas = ref(null)
let scene, camera, renderer, geometry, material, mesh, controls, clock;

const width = window.innerWidth
const height = window.innerHeight

onMounted(()=>{
  // 场景
  scene = new THREE.Scene();
  
  // 相机
  camera = new THREE.PerspectiveCamera(75, width/height, 0.1, 1000);
  camera.position.set(0, 0, 20);

  // 渲染
  renderer = new THREE.WebGL1Renderer({
    antialias: true
  });
  renderer.setSize(width, height);
  renderer.setClearColor("#000")
  scene.background = new THREE.Color("#ccc")
  scene.environment = new THREE.Color("#ccc")
  domCanvas.value.appendChild(renderer.domElement );

  // 添加几何体
  geometry = new THREE.SphereGeometry( 15, 32, 16 );
  material = new THREE.MeshBasicMaterial( { color: 0xffff00 } );
  mesh = new THREE.Mesh( geometry, material );
  scene.add(mesh)

  // 轨道控制
  controls = new OrbitControls(camera, renderer.domElement);
  controls.update();
  controls.keys = {
    LEFT: 'ArrowLeft', //left arrow
    UP: 'ArrowUp', // up arrow
    RIGHT: 'ArrowRight', // right arrow
    BOTTOM: 'ArrowDown' // down arrow
  }

  console.log("===")
  clock = new THREE.Clock()

  animat();
})

const animat = () => {
  requestAnimationFrame(animat);
  renderer.render(scene, camera);
}

</script>
<style scoped>
</style>
