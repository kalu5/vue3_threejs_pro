<template>
  <div class="house-wrap" ref="houseRef">
    HOUSE_3D 
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

import * as THREE from 'three';

import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

// 创建场景
const scene = new THREE.Scene();

// 创建相机
const camera = new THREE.PerspectiveCamera(75, 
window.innerWidth / window.innerHeight,
0.1,
1000
)

// 设置相机位置
camera.position.set(0, 0, 0.1)

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

// 添加立方体
const geometry = new THREE.BoxGeometry(10, 10, 10)
// 基础材质
const material = new THREE.MeshBasicMaterial({
  color: 0x00ff00
})

// 创建纹理材质
const livingArr = ['4_l', '4_r', '4_u', '4_d', '4_b', '4_f']
const boxMaterials = []

livingArr.forEach(live => {
  // 加载纹理
  const texture = new THREE.TextureLoader().load(`/imgs/living/${live}.jpg`)
  if(['4_u', '4_d'].includes(live)) {
    texture.rotation = Math.PI;
    texture.center = new THREE.Vector2(.5, .5)
  }
  boxMaterials.push(new THREE.MeshBasicMaterial({map: texture}))
})

// 创建物体
const cube =  new THREE.Mesh(geometry, boxMaterials)
// 设置物理比例 进入立方体内
cube.geometry.scale(1, 1, -1)
// 添加到场景中
scene.add(cube)

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

<style lang="less" scoped></style>