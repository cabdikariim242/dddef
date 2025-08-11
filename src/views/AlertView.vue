<template>
  <div class="flex items-center justify-center min-h-screen bg-gradient-to-r from-purple-700 via-pink-600 to-red-500 p-8">
    <button
      @mouseenter="onHover"
      @mouseleave="onLeave"
      @mousedown="onClick"
      @mouseup="onRelease"
      class="relative overflow-hidden px-16 py-4 text-white font-semibold text-xl rounded-full shadow-lg cursor-pointer"
      style="width: 220px; height: 60px;"
    >
      <svg
        ref="svg"
        viewBox="0 0 220 60"
        class="absolute top-0 left-0 w-full h-full"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          ref="path"
          :d="pathD"
          fill="rgba(255 255 255 / 0.25)"
          stroke="white"
          stroke-width="2"
        />
      </svg>
      <span class="relative z-10 select-none">Liquid Button</span>
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { gsap } from "gsap";

const svg = ref(null);
const path = ref(null);

const basePath =
  "M10,30 Q30,10 60,10 Q90,10 110,30 Q130,50 160,50 Q190,50 210,30 Q190,10 160,10 Q130,10 110,30 Q90,50 60,50 Q30,50 10,30 Z";
const hoverPath =
  "M10,30 Q40,5 70,20 Q100,35 130,10 Q160,50 190,40 Q210,30 190,10 Q160,10 130,50 Q100,40 70,60 Q40,45 10,30 Z";

const clickPath =
  "M10,30 Q35,0 70,15 Q105,30 140,5 Q175,50 210,30 Q190,10 160,10 Q130,50 100,40 Q70,60 40,45 10,30 Z";

const pathD = ref(basePath);

// function animatePath(to) {
//   gsap.to(pathD, {
//     duration: 0.5,
//     ease: "power2.out",
//     onUpdate() {
//       path.value.setAttribute("d", pathD.value);
//     },
//     value: to,
//   });
//}

function onHover() {
  gsap.to(pathD, {
    duration: 0.7,
    ease: "elastic.out(1, 0.5)",
    onUpdate() {
      path.value.setAttribute("d", pathD.value);
    },
    value: hoverPath,
  });
}

function onLeave() {
  gsap.to(pathD, {
    duration: 0.8,
    ease: "elastic.out(1, 0.3)",
    onUpdate() {
      path.value.setAttribute("d", pathD.value);
    },
    value: basePath,
  });
}

function onClick() {
  gsap.to(pathD, {
    duration: 0.3,
    ease: "power3.inOut",
    onUpdate() {
      path.value.setAttribute("d", pathD.value);
    },
    value: clickPath,
  });
}

function onRelease() {
  onHover();
}
</script>

<style scoped>
button {
  backdrop-filter: blur(12px);
  background: rgba(255 255 255 / 0.1);
  transition: background-color 0.3s ease;
  user-select: none;
}
button:hover {
  background: rgba(255 255 255 / 0.15);
}
</style>
  