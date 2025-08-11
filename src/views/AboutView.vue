<template>
  <div
    class="tag-cloud"
    @mousedown.prevent="startDrag"
    @touchstart.prevent="startDrag"
    @mouseup="stopDrag"
    @mouseleave="stopDrag"
    @touchend="stopDrag"
    @mousemove="onDrag"
    @touchmove="onDrag"
  >
    <div
      v-for="(tag, i) in tags"
      :key="i"
      class="tag"
      :style="getTagStyle(tag)"
      @mouseenter="hoverIndex = i"
      @mouseleave="hoverIndex = null"
    >
      {{ tag.text }}
      <transition name="fade">
        <div v-if="hoverIndex === i" class="tooltip">{{ tag.tooltip }}</div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: "TagCloud3D",
  data() {
    return {
      tags: [
        { text: "Elegance", tooltip: "Graceful simplicity and style" },
        { text: "Innovation", tooltip: "Creating the new and exciting" },
        { text: "Creativity", tooltip: "Boundless imagination at work" },
        { text: "Harmony", tooltip: "Balanced and pleasing arrangement" },
        { text: "Inspiration", tooltip: "A spark that fuels passion" },
        { text: "Passion", tooltip: "Intense enthusiasm and love" },
        { text: "Focus", tooltip: "Directed attention and energy" },
        { text: "Flow", tooltip: "Effortless progress and rhythm" },
        { text: "Balance", tooltip: "Stable and poised state" },
        { text: "Vision", tooltip: "Clear and ambitious goals" },
        { text: "Simplicity", tooltip: "Less is more" },
        { text: "Impact", tooltip: "Making a powerful difference" },
        { text: "Purpose", tooltip: "Meaningful direction" },
        { text: "Depth", tooltip: "Profound insight and substance" },
        { text: "Grace", tooltip: "Elegance in movement and thought" },
        { text: "Zen", tooltip: "Calm mindfulness" },
        { text: "Spark", tooltip: "A bright beginning" },
        { text: "Momentum", tooltip: "Sustained energy" },
        { text: "Flow", tooltip: "Seamless motion" },
        { text: "Clarity", tooltip: "Pure understanding" },
      ],
      radius: 160,
      rotationX: 0,
      rotationY: 0,
      velocityX: 0,
      velocityY: 0,
      dragging: false,
      lastX: 0,
      lastY: 0,
      hoverIndex: null,
      width: window.innerWidth,
      height: window.innerHeight,
      animationFrameId: null,
    };
  },
  methods: {
    initTags() {
      // Distribute points on sphere via Fibonacci sphere algorithm
      const N = this.tags.length;
      const goldenAngle = Math.PI * (3 - Math.sqrt(5));
      this.tags.forEach((tag, i) => {
        const y = 1 - (i / (N - 1)) * 2; // y goes from 1 to -1
        const radius = Math.sqrt(1 - y * y);
        const theta = goldenAngle * i;
        tag.x = Math.cos(theta) * radius;
        tag.y = y;
        tag.z = Math.sin(theta) * radius;
      });
    },
    rotate3D() {
      // Rotate tags around X and Y axis by rotationX, rotationY
      const sinX = Math.sin(this.rotationX);
      const cosX = Math.cos(this.rotationX);
      const sinY = Math.sin(this.rotationY);
      const cosY = Math.cos(this.rotationY);

      this.tags.forEach((tag) => {
        // Rotate around X axis
        let y1 = tag.y * cosX - tag.z * sinX;
        let z1 = tag.y * sinX + tag.z * cosX;

        // Rotate around Y axis
        let x2 = tag.x * cosY + z1 * sinY;
        let z2 = -tag.x * sinY + z1 * cosY;

        tag.screenX = x2 * this.radius + this.width / 2;
        tag.screenY = y1 * this.radius + this.height / 2;

        // Perspective scale & opacity based on z-depth
        const depth = (z2 + 1) / 2; // 0 (far) to 1 (near)
        tag.scale = 0.5 + depth * 0.9; // scale from 0.5 to 1.4
        tag.opacity = 0.3 + depth * 0.7; // opacity from 0.3 to 1
        tag.zIndex = Math.floor(depth * 1000);
      });
    },
    getTagStyle(tag) {
      return {
        position: "absolute",
        left: `${tag.screenX}px`,
        top: `${tag.screenY}px`,
        transform: `translate(-50%, -50%) scale(${tag.scale})`,
        opacity: tag.opacity,
        cursor: "default",
        color: "#aad4ff",
        fontWeight: "600",
        userSelect: "none",
        pointerEvents: "auto",
        zIndex: tag.zIndex,
        textShadow:
          this.hoverIndex !== null && this.tags[this.hoverIndex] === tag
            ? "0 0 15px #89c7ff, 0 0 25px #4a9fff"
            : "none",
        transition: "transform 0.3s ease, opacity 0.3s ease, text-shadow 0.3s ease",
      };
    },
    animate() {
      if (!this.dragging) {
        this.velocityX *= 0.95;
        this.velocityY *= 0.95;
        this.rotationX += this.velocityY;
        this.rotationY += this.velocityX;
      }
      this.rotate3D();
      this.animationFrameId = requestAnimationFrame(this.animate);
    },
    startDrag(event) {
      this.dragging = true;
      this.lastX = event.touches ? event.touches[0].clientX : event.clientX;
      this.lastY = event.touches ? event.touches[0].clientY : event.clientY;
    },
    onDrag(event) {
      if (!this.dragging) return;
      const currentX = event.touches ? event.touches[0].clientX : event.clientX;
      const currentY = event.touches ? event.touches[0].clientY : event.clientY;
      const deltaX = currentX - this.lastX;
      const deltaY = currentY - this.lastY;
      this.velocityX = deltaX * 0.005;
      this.velocityY = deltaY * 0.005;
      this.rotationX += this.velocityY;
      this.rotationY += this.velocityX;
      this.lastX = currentX;
      this.lastY = currentY;
    },
    stopDrag() {
      this.dragging = false;
    },
    handleResize() {
      this.width = window.innerWidth;
      this.height = window.innerHeight;
    },
  },
  mounted() {
    this.initTags();
    this.rotate3D();
    this.animate();
    window.addEventListener("resize", this.handleResize);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.handleResize);
    cancelAnimationFrame(this.animationFrameId);
  },
};
</script>

<style scoped>
.tag-cloud {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(135deg, #0d0d2a, #121b4f, #060615);
  overflow: hidden;
  user-select: none;
  cursor: grab;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: #aad4ff;
  perspective: 1000px;
}
.tag-cloud:active {
  cursor: grabbing;
}
.tag {
  position: absolute;
  white-space: nowrap;
  will-change: transform, opacity;
  transition: box-shadow 0.3s ease;
  font-size: 1.25rem;
  padding: 2px 6px;
  border-radius: 6px;
  text-shadow: 0 0 6px #5577aaaa;
  user-select: none;
  pointer-events: auto;
}
.tooltip {
  position: absolute;
  top: 140%;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(10, 10, 30, 0.9);
  padding: 6px 12px;
  border-radius: 12px;
  font-size: 0.85rem;
  color: #c0d8ff;
  white-space: normal;
  pointer-events: none;
  user-select: text;
  box-shadow: 0 0 20px #2b3c7d;
  max-width: 220px;
  text-align: center;
  opacity: 0.9;
  z-index: 1000;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
