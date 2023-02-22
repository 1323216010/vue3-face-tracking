<script setup>
import * as THREE from 'https://unpkg.com/three@0.147.0/build/three.module.js';
import { MindARThree } from 'https://cdn.jsdelivr.net/npm/mind-ar@1.2.0/dist/mindar-face-three.prod.js';
import { ref, onMounted } from 'vue';

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
</script>

<template>
  <div style="text-align: center">
    <a>手机在QQ浏览器测试</a>
    <div>
      <button @click="f('/p1')">图案一</button>
      <button @click="f('/p2')">图案二</button>
      <button @click="f('/p3')">图案三</button>
      <button @click="f('/p4')">图案四</button>
    </div>
    <div id="container"></div>
  </div>
</template>

<style scoped>
body {
  margin: 0;
}

#container {
  width: 100vw;
  height: 100vh;
  position: relative;
  overflow: hidden;
}
</style>
