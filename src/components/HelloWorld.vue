<script setup>
import * as THREE from 'https://unpkg.com/three@0.147.0/build/three.module.js';
import { MindARThree } from 'https://cdn.jsdelivr.net/npm/mind-ar@1.2.0/dist/mindar-face-three.prod.js';
import { onMounted } from 'vue';

onMounted(() => {
});
function f(path) {
  const mindarThree = new MindARThree({
    container: document.querySelector('#container'),
  });
  const { renderer, scene, camera } = mindarThree;

  const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
  scene.add(light);

  const faceMesh = mindarThree.addFaceMesh();

  const texture = new THREE.TextureLoader().load(path);
    faceMesh.material.map = texture;
    faceMesh.material.transparent = true;
    faceMesh.material.needsUpdate = true;
    scene.add(faceMesh);

    const start = async () => {
      await mindarThree.start();
      renderer.setAnimationLoop(() => {
        renderer.render(scene, camera);
      });
    };
    start();
}
function g() {
  console.log('g函数执行。。。')
}
</script>

<template>
  <div style="text-align: center">
    <div id="container" style="width:70%;height: 95%;display:inline-block;"></div>

    <p style="text-align:center">
      <button click="f('/p1')">图案一</button>
      <button click="f('/p2')">图案二</button>
      <button click="f('/p3')">图案三</button>
      <button click="f('/p4')">图案四</button>
      <button click="g()">测试</button>
      <!-- <a onclick="f()">55555555555555</a> -->
    </p>
  </div>
</template>

<style scoped></style>
