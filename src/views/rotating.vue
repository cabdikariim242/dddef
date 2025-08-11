# Ignore node_modules folder
<template>
  <div class="relative w-screen h-screen bg-gradient-to-br from-black via-indigo-900 to-black overflow-hidden select-none">
    <canvas ref="canvas" class="w-full h-full block"></canvas>

    <div
      class="absolute top-6 left-6 bg-black bg-opacity-60 rounded-lg p-5 text-white max-w-sm space-y-4 shadow-lg"
    >
      <h1 class="text-2xl font-bold mb-3">Interactive Galaxy</h1>

      <label class="flex flex-col">
        Rotation Speed: <span>{{ rotationSpeed.toFixed(3) }}</span>
        <input
          type="range"
          min="0"
          max="0.05"
          step="0.001"
          v-model.number="rotationSpeed"
          class="mt-1"
        />
      </label>

      <label class="flex flex-col">
        Star Count: <span>{{ starCount }}</span>
        <input
          type="range"
          min="100"
          max="2000"
          step="50"
          v-model.number="starCount"
          @change="rebuildStars"
          class="mt-1"
        />
      </label>

      <label class="flex flex-col">
        Zoom: <span>{{ camera.position.z.toFixed(1) }}</span>
        <input
          type="range"
          min="10"
          max="100"
          step="1"
          v-model.number="cameraZ"
          @input="updateCameraZ"
          class="mt-1"
        />
      </label>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import * as THREE from "three";

const canvas = ref(null);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
let renderer;

const rotationSpeed = ref(0.01);
const starCount = ref(1000);
const cameraZ = ref(50);

let starsGeometry, starsMaterial, starsPoints;

let isDragging = false;
let previousMouseX = 0;
let rotationVelocity = 0;

function createStars() {
  if (starsPoints) {
    scene.remove(starsPoints);
    starsGeometry.dispose();
    starsMaterial.dispose();
  }

  starsGeometry = new THREE.BufferGeometry();
  const positions = [];

  for (let i = 0; i < starCount.value; i++) {
    // Random stars in a sphere
    const r = THREE.MathUtils.randFloatSpread(200);
    const theta = THREE.MathUtils.randFloatSpread(2 * Math.PI);
    const phi = THREE.MathUtils.randFloatSpread(Math.PI);

    const x = r * Math.sin(phi) * Math.cos(theta);
    const y = r * Math.sin(phi) * Math.sin(theta);
    const z = r * Math.cos(phi);

    positions.push(x, y, z);
  }

  starsGeometry.setAttribute(
    "position",
    new THREE.Float32BufferAttribute(positions, 3)
  );

  starsMaterial = new THREE.PointsMaterial({
    color: 0xffffff,
    size: 0.7,
    sizeAttenuation: true,
    transparent: true,
    opacity: 0.8,
  });

  starsPoints = new THREE.Points(starsGeometry, starsMaterial);
  scene.add(starsPoints);
}

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}

function animate() {
  if (!isDragging) {
    starsPoints.rotation.y += rotationSpeed.value + rotationVelocity;
    rotationVelocity *= 0.95; // friction slow down
  }

  renderer.render(scene, camera);
  requestAnimationFrame(animate);
}

function onMouseDown(e) {
  isDragging = true;
  previousMouseX = e.clientX;
}

function onMouseMove(e) {
  if (!isDragging) return;
  const deltaX = e.clientX - previousMouseX;
  rotationVelocity = deltaX * 0.005;
  starsPoints.rotation.y += rotationVelocity;
  previousMouseX = e.clientX;
}

function onMouseUp() {
  isDragging = false;
}

function onWheel(e) {
  cameraZ.value -= e.deltaY * 0.05;
  cameraZ.value = Math.min(Math.max(cameraZ.value, 10), 100);
  camera.position.z = cameraZ.value;
}

function rebuildStars() {
  createStars();
}

function updateCameraZ() {
  camera.position.z = cameraZ.value;
}

onMounted(() => {
  renderer = new THREE.WebGLRenderer({ canvas: canvas.value, antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);

  camera.position.z = cameraZ.value;

  createStars();
  animate();

  window.addEventListener("resize", onWindowResize);
  window.addEventListener("mousedown", onMouseDown);
  window.addEventListener("mousemove", onMouseMove);
  window.addEventListener("mouseup", onMouseUp);
  window.addEventListener("wheel", onWheel);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", onWindowResize);
  window.removeEventListener("mousedown", onMouseDown);
  window.removeEventListener("mousemove", onMouseMove);
  window.removeEventListener("mouseup", onMouseUp);
  window.removeEventListener("wheel", onWheel);

  if (renderer) {
    renderer.dispose();
  }
});
</script>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
  background: black;
}
</style>
