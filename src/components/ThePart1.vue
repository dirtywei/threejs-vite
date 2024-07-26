<template>
  <canvas ref="canvasRef"></canvas>
</template>

<script setup>
import * as THREE from 'three'
const canvasRef = ref()

onMounted(() => {
  // 定义渲染尺寸
  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
  }

  // 初始化渲染器
  const renderer = new THREE.WebGLRenderer({ canvas: canvasRef.value })
  renderer.setSize(sizes.width, sizes.height)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

  // 初始化场景
  const scene = new THREE.Scene()

  // 初始化相机
  const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
  camera.position.z = 3
  scene.add(camera)

  //页面大小变化时，更新渲染器和相机
  window.addEventListener('resize', () => {
    sizes.width = window.innerWidth
    sizes.height = window.innerHeight

    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

    camera.aspect = sizes.width / sizes.height
    camera.updateProjectionMatrix()
  })

  // 初始化几何体
  const geometry = new THREE.BoxGeometry(1, 1, 1)
  const material = new THREE.MeshBasicMaterial({ color: 0x03c03c })
  const mesh = new THREE.Mesh(geometry, material)
  scene.add(mesh)

  // 动画
  const animate = () => {
    // 给网格模型添加一个转动动画
    mesh.rotation.x += 0.01
    mesh.rotation.y += 0.01
    // 更新渲染器
    renderer.render(scene, camera)
  }
  renderer.setAnimationLoop(animate)
})
</script>

<style scoped></style>
