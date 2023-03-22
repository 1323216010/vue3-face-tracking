<script setup>
import * as THREE from '../utils/three.module.js';
import { MindARThree } from '../utils/ar/three.js';
import { ref, onMounted } from 'vue';
import eruda from 'eruda'

const open = ref(false)

onMounted(() => {
});
var mindarThree
function f(path) {
  open.value = true
  if(mindarThree) {
    mindarThree.stop();
  }
  mindarThree = new MindARThree({
    container: document.querySelector('#container'),
  });
  const { renderer, scene, camera } = mindarThree;

  const light = new THREE.HemisphereLight(0xffffff, 0xbbbbff, 1);
  scene.add(light);

  const faceMesh = mindarThree.addFaceMesh();

  const texture = new THREE.TextureLoader().load(path);
  console.log(texture)
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
eruda.init();
</script>

<template>
  <div style="text-align: center">
    <div>
      <button class="btn btn-primary" @click="f('/p1')">图案一</button>&thinsp;
      <button class="btn btn-primary" @click="f('/p2')">图案二</button>&thinsp;
      <button class="btn btn-primary" @click="f('/p3')">图案三</button>&thinsp;
      <button class="btn btn-primary" @click="f('/p4')">图案四</button>
    </div>
    <div id="container" v-show="open"></div>

    <a>目前只能在指定浏览器体验</a>
    <br>
    <a>安卓为自带浏览器或QQ浏览器，苹果为自带浏览器，Windows为谷歌浏览器</a>
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

.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
  border-radius: 4px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
</style>
