<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button v-if="showControls" @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Anterior">
      <div :class="$style.btnGlow" />
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button v-if="showControls" @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Siguiente">
      <div :class="$style.btnGlow" />
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="m9 18 6-6 6-6"/></svg>
    </button>

    <div 
      ref="carouselRef"
      :class="[$style.carouselRoot, { [$style.isDragging]: isDragging }]" 
      @mousedown="startDragging"
      @mousemove="onDragging"
      @mouseup="stopDragging"
      @mouseleave="stopDragging"
    >
      <div :class="$style.track">
        <div 
          v-for="(item, index) in items" 
          :key="index" 
          :class="$style.slide"
          :style="{ '--slide-aspect': item.aspectRatio || defaultAspectRatio }"
        >
          <div :class="$style.cyberFrame" @click="$emit('select', item)">
            <div :class="$style.cornerTop" />
            <div :class="$style.cornerBottom" />
            <div v-if="showScanline" :class="$style.scanline" />

            <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            
            <div v-if="item.label" :class="$style.cyberLabel">
              <span :class="$style.glitchText">
                {{ item.label }}
              </span>
              <div :class="$style.statusDot" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { MediaRender } from '../renderers';

interface CarouselItem {
  url: string;
  alt?: string;
  label?: string;
  aspectRatio?: string;
}

interface Props {
  items: CarouselItem[];
  defaultAspectRatio?: string;
  gap?: string;
  accentColor?: string; // Neon color
  bgColor?: string;     // Deep background
  cardHeight?: string;
  scanSpeed?: string;   // e.g., '4s'
  showControls?: boolean;
  showScanline?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  defaultAspectRatio: '1/1',
  gap: '20px',
  accentColor: '#00f3ff',
  bgColor: '#0a0a0c',
  cardHeight: '400px',
  scanSpeed: '4s',
  showControls: true,
  showScanline: true
});

defineEmits(['select']);

const carouselRef = ref<HTMLElement | null>(null);
const isDragging = ref(false);
let startX: number, scrollLeft: number;

const scroll = (dir: 'next' | 'prev') => {
  if (!carouselRef.value) return;
  const scrollAmount = carouselRef.value.clientWidth * 0.75;
  carouselRef.value.scrollBy({ 
    left: dir === 'next' ? scrollAmount : -scrollAmount, 
    behavior: 'smooth' 
  });
};

const startDragging = (e: MouseEvent) => {
  isDragging.value = true;
  startX = e.pageX - (carouselRef.value?.offsetLeft || 0);
  scrollLeft = carouselRef.value?.scrollLeft || 0;
};
const stopDragging = () => { isDragging.value = false; };
const onDragging = (e: MouseEvent) => {
  if (!isDragging.value || !carouselRef.value) return;
  e.preventDefault();
  const x = e.pageX - (carouselRef.value.offsetLeft || 0);
  carouselRef.value.scrollLeft = scrollLeft - (x - startX) * 1.5;
};

const carouselStyles = computed(() => ({
  '--cyber-neon': props.accentColor,
  '--cyber-bg': props.bgColor,
  '--c-gap': props.gap,
  '--c-height': props.cardHeight,
  '--scan-speed': props.scanSpeed
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  background: var(--cyber-bg);
  display: flex;
  align-items: center;
  overflow: hidden;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 60px 0;
  cursor: crosshair;
  scrollbar-width: none;
}

.carouselRoot::-webkit-scrollbar { display: none; }

.isDragging {
  scroll-snap-type: none;
  cursor: grabbing;
}

.track {
  display: flex;
  gap: var(--c-gap);
  width: max-content;
  padding: 0 100px;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: var(--c-height);
  scroll-snap-align: center;
}

/* --- HUD Navigation --- */
.navBtn {
  position: absolute;
  z-index: 100;
  top: 50%;
  transform: translateY(-50%);
  width: 50px;
  height: 50px;
  background: transparent;
  color: var(--cyber-neon);
  border: 1px solid var(--cyber-neon);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  clip-path: polygon(0 20%, 20% 0, 100% 0, 100% 80%, 80% 100%, 0 100%);
  transition: all 0.2s ease;
}

.btnGlow {
  position: absolute;
  inset: 0;
  background: var(--cyber-neon);
  opacity: 0.1;
  transition: opacity 0.2s;
}

.navBtn:hover {
  background: var(--cyber-neon);
  color: #000;
  box-shadow: 0 0 20px var(--cyber-neon);
}

.navBtn:hover .btnGlow { opacity: 0.4; }

.prevBtn { left: 25px; }
.nextBtn { right: 25px; }

/* --- Cyber Frame --- */
.cyberFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: #000;
  border: 1px solid var(--cyber-neon);
  clip-path: polygon(0 0, 92% 0, 100% 8%, 100% 100%, 8% 100%, 0 92%);
  overflow: hidden;
  box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.5);
  cursor: pointer;
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.6;
  filter: contrast(1.2) grayscale(0.5);
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
}

.slide:hover .media {
  opacity: 1;
  filter: contrast(1.1) grayscale(0);
  transform: scale(1.08);
}

/* HUD Ornaments */
.cornerTop {
  position: absolute;
  top: 0; right: 0;
  width: 30px; height: 30px;
  border-top: 3px solid var(--cyber-neon);
  border-right: 3px solid var(--cyber-neon);
  z-index: 2;
  pointer-events: none;
}

.cornerBottom {
  position: absolute;
  bottom: 0; left: 0;
  width: 15px; height: 15px;
  border-bottom: 2px solid var(--cyber-neon);
  border-left: 2px solid var(--cyber-neon);
  z-index: 2;
  pointer-events: none;
}

.scanline {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 2px;
  background: linear-gradient(to right, transparent, var(--cyber-neon), transparent);
  opacity: 0.3;
  animation: scan var(--scan-speed) linear infinite;
  z-index: 3;
  pointer-events: none;
}

@keyframes scan {
  0% { transform: translateY(0); }
  100% { transform: translateY(var(--c-height)); }
}

.cyberLabel {
  position: absolute;
  bottom: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.85);
  border-left: 3px solid var(--cyber-neon);
  padding: 6px 15px;
  display: flex;
  align-items: center;
  gap: 12px;
  backdrop-filter: blur(5px);
  z-index: 10;
}

.glitchText {
  color: var(--cyber-neon);
  font-family: 'Courier New', Courier, monospace;
  font-weight: 900;
  font-size: 0.75rem;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.statusDot {
  width: 6px;
  height: 6px;
  background: var(--cyber-neon);
  border-radius: 50%;
  box-shadow: 0 0 8px var(--cyber-neon);
  animation: blink 1.5s infinite;
}

@keyframes blink {
  50% { opacity: 0.1; }
}

@media (max-width: 768px) {
  .slide { height: calc(var(--c-height) * 0.8); }
  .navBtn { display: none; }
}
</style>