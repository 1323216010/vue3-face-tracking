<script setup>
import * as THREE from 'https://unpkg.com/three@0.147.0/build/three.module.js';
import { MindARThree } from 'https://cdn.jsdelivr.net/npm/mind-ar@1.2.0/dist/mindar-face-three.prod.js';
import { onMounted } from 'vue';

onMounted(() => {
  const mindarThree = new MindARThree({
    container: document.querySelector('#container'),
  });
  const { renderer, scene, camera } = mindarThree;

  const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
  scene.add(light);

  const faceMesh = mindarThree.addFaceMesh();

  function f1() {
    const texture = new THREE.TextureLoader().load(
      '/p1'
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
  }

});
</script>

<template>
  <div style="text-align: center">
    <div id="container" style="width:70%;height: 95%;display:inline-block;"></div>

    <p style="text-align:center">
      <button click="f1()">图案一</button>
      <button>图案二</button>
      <button>图案三</button>
      <button>图案四</button>
      <!-- <a onclick="f()">55555555555555</a> -->
    </p>
  </div>
</template>

<style scoped></style>
