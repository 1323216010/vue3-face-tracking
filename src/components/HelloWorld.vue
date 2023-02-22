<script setup>
import * as THREE from 'https://unpkg.com/three@0.147.0/build/three.module.js';
import { MindARThree } from 'https://cdn.jsdelivr.net/npm/mind-ar@1.2.0/dist/mindar-face-three.prod.js';
import { ref, onMounted } from 'vue';

defineProps({
  msg: String,
});

const count = ref(0);

onMounted(() => {
  const mindarThree = new MindARThree({
    container: document.querySelector('#container'),
  });
  const { renderer, scene, camera } = mindarThree;

  const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
  scene.add(light);

  const faceMesh = mindarThree.addFaceMesh();

  const texture = new THREE.TextureLoader().load(
    'https://7072-prod-4gxasl3i1c9bec5c-1316782094.tcb.qcloud.la/test/p1.png?sign=b1bba8356e63698956788d4315f6d17f&t=1677056739'
  );
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
});
</script>

<template>
  <div style="text-align: center">
    <div
      id="container"
      style="width: 70%; height: 45%; display: inline-block"
    ></div>
  </div>
  <h1>{{ msg }}</h1>
</template>

<style scoped></style>
