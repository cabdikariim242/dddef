<template>
  <div class="canvas" :style="bgStyle" @mousemove="onMouseMove" @mouseleave="onMouseLeave">
    <div
      v-for="(word, index) in words"
      :key="index"
      class="floating-word"
      :style="getWordStyle(word)"
      @mouseenter="hoverIndex = index"
      @mouseleave="hoverIndex = null"
    >
      {{ word.text }}
      <transition name="fade">
        <div v-if="hoverIndex === index" class="tooltip">{{ word.tooltip }}</div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: "FloatingWordsArt",
  data() {
    return {
      words: [
        { text: "Elegance", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Graceful simplicity and style" },
        { text: "Flow", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Natural movement without effort" },
        { text: "Light", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Illumination and clarity" },
        { text: "Space", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Room to breathe and create" },
        { text: "Whisper", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Soft subtle communication" },
        { text: "Pulse", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Rhythm of life" },
        { text: "Glide", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Smooth effortless motion" },
        { text: "Breathe", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Calm and presence" },
        { text: "Shine", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Radiate confidence" },
        { text: "Dream", x: 0, y: 0, vx: 0, vy: 0, tooltip: "Imagining the impossible" },
      ],
      hoverIndex: null,
      mouseX: null,
      mouseY: null,
      width: window.innerWidth,
      height: window.innerHeight,
      animationFrameId: null,
    };
  },
  computed: {
    bgStyle() {
      return {
        background:
          "linear-gradient(135deg, #0f2027, #203a43, #2c5364)",
      };
    },
  },
  methods: {
    initWords() {
      this.words.forEach((w) => {
        w.x = Math.random() * this.width;
        w.y = Math.random() * this.height;
        w.vx = (Math.random() - 0.5) * 0.5;
        w.vy = (Math.random() - 0.5) * 0.5;
      });
    },
    animate() {
      this.words.forEach((w) => {
        // Move word
        w.x += w.vx;
        w.y += w.vy;

        // Bounce off edges
        if (w.x < 0 || w.x > this.width) w.vx = -w.vx;
        if (w.y < 0 || w.y > this.height) w.vy = -w.vy;

        // Apply gentle friction
        w.vx *= 0.99;
        w.vy *= 0.99;

        // Repel from mouse when hovering
        if (this.mouseX !== null && this.mouseY !== null) {
          const dx = w.x - this.mouseX;
          const dy = w.y - this.mouseY;
          const dist = Math.sqrt(dx * dx + dy * dy);
          const minDist = 100;
          if (dist < minDist && dist > 0) {
            const force = (minDist - dist) / minDist;
            w.vx += (dx / dist) * force * 0.5;
            w.vy += (dy / dist) * force * 0.5;
          }
        }
      });
      this.animationFrameId = requestAnimationFrame(this.animate);
    },
    getWordStyle(word) {
      const scale = this.hoverIndex !== null && this.words[this.hoverIndex] === word ? 1.6 : 1;
      const glow = this.hoverIndex !== null && this.words[this.hoverIndex] === word ? "0 0 15px #aad4ff" : "none";
      return {
        position: "absolute",
        left: `${word.x}px`,
        top: `${word.y}px`,
        transform: `translate(-50%, -50%) scale(${scale})`,
        color: "#aad4ff",
        cursor: "default",
        fontWeight: "600",
        fontSize: "1.2rem",
        userSelect: "none",
        textShadow: glow,
        transition: "transform 0.3s ease, text-shadow 0.3s ease",
        whiteSpace: "nowrap",
        pointerEvents: "auto",
      };
    },
    onMouseMove(e) {
      this.mouseX = e.clientX;
      this.mouseY = e.clientY;
    },
    onMouseLeave() {
      this.mouseX = null;
      this.mouseY = null;
    },
    handleResize() {
      this.width = window.innerWidth;
      this.height = window.innerHeight;
    },
  },
  mounted() {
    this.initWords();
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
.canvas {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  position: relative;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: #aad4ff;
  user-select: none;
  background-size: 400% 400%;
  animation: bgPulse 20s ease-in-out infinite;
}

@keyframes bgPulse {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.floating-word {
  position: absolute;
  will-change: transform;
}

.tooltip {
  position: absolute;
  top: 150%;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(10, 20, 40, 0.85);
  padding: 6px 12px;
  border-radius: 12px;
  font-size: 0.85rem;
  color: #c0d8ff;
  white-space: normal;
  pointer-events: none;
  user-select: text;
  box-shadow: 0 0 10px #5577aa99;
  max-width: 220px;
  text-align: center;
  opacity: 0.9;
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
