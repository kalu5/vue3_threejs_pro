<template>
  <div class="house-wrap" ref="houseRef">
    HOUSE_3D hdr
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

import * as THREE from 'three';

import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader'

// 创建场景
const scene = new THREE.Scene();

// 创建相机
const camera = new THREE.PerspectiveCamera(75, 
window.innerWidth / window.innerHeight,
0.1,
1000
)

// 设置相机位置
camera.position.z = 0.1

// 创建渲染器
const renderer = new THREE.WebGLRenderer()

// 渲染器设置宽高
renderer.setSize(window.innerWidth, window.innerHeight)

// 绑定dom
const houseRef = ref<HTMLDivElement | null>(null)

// 渲染函数，每一帧都会重新渲染
const render = () => {
  renderer.render(scene, camera)
  requestAnimationFrame(render)
}

// 添加圆
const geometry = new THREE.SphereGeometry(5, 32, 32)

// rgbe加载器
const loader = new RGBELoader();
loader.load('/imgs/hdr/Living.hdr', (texture) => {
  const material = new THREE.MeshBasicMaterial({
    map: texture
  })
  const sphere = new THREE.Mesh(geometry, material)
  sphere.geometry.scale(1, 1, -1)
  // 添加到场景中
  scene.add(sphere)
})

onMounted(() => {
  // 添加轨道控制器
  const controls = new OrbitControls( camera, houseRef.value );
  // 轨道控制器设置阻尼
  controls.enableDamping = true;

  // 将canvas添加到当前dom
  houseRef.value?.appendChild(renderer.domElement)
  // 渲染
  render()
})


</script>

<style scoped>
.house-wrap  {
  width: 100%;
  height: 100%;
}
</style>