<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button v-if="showControls" @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Anterior">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button v-if="showControls" @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Siguiente">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
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
          <div :class="$style.speedFrame" @click="$emit('select', item)">
            <template v-if="showEffects">
              <div :class="$style.speedLine" style="top: 20%; --d: 0.1s" />
              <div :class="$style.speedLine" style="top: 50%; --d: 0.5s" />
              <div :class="$style.speedLine" style="top: 80%; --d: 0.3s" />
            </template>

            <div :class="$style.imageSkew">
              <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            </div>
            
            <div v-if="item.label" :class="$style.labelContainer">
              <div :class="$style.italicBadge">
                <span :class="$style.fastText">{{ item.label }}</span>
              </div>
              <div :class="$style.velocityBar" />
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
  accentColor?: string;     // The "Racing" color
  baseColor?: string;       // Background behind the skew
  cardHeight?: string;
  skewAngle?: number;       // Degree of lean (default 4)
  showEffects?: boolean;    // Toggle speed lines
  showControls?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  defaultAspectRatio: '1/1',
  gap: '12px',
  accentColor: '#ff003c',
  baseColor: '#0f0f0f',
  cardHeight: '420px',
  skewAngle: 4,
  showEffects: true,
  showControls: true
});

defineEmits(['select']);

const carouselRef = ref<HTMLElement | null>(null);
const isDragging = ref(false);
let startX: number, scrollLeft: number;

const scroll = (dir: 'next' | 'prev') => {
  if (!carouselRef.value) return;
  const scrollAmount = carouselRef.value.clientWidth * 0.8;
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
  '--c-accent': props.accentColor,
  '--c-base': props.baseColor,
  '--c-gap': props.gap,
  '--c-height': props.cardHeight,
  '--skew-deg': `${props.skewAngle}deg`,
  '--skew-deg-neg': `-${props.skewAngle}deg`,
  '--skew-btn': `-${props.skewAngle * 3}deg`,
  '--skew-btn-rev': `${props.skewAngle * 3}deg`,
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 40px 0;
  cursor: e-resize;
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
  padding: 0 60px;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: var(--c-height);
  scroll-snap-align: start;
  transform: skewX(var(--skew-deg-neg));
  transition: transform 0.3s ease;
}

.speedFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: var(--c-base);
  border-right: 6px solid var(--c-accent);
  overflow: hidden;
  box-shadow: 20px 0 50px rgba(0,0,0,0.5);
  cursor: pointer;
}

.imageSkew {
  width: 140%; 
  height: 100%;
  margin-left: -20%;
  transform: skewX(var(--skew-deg));
  overflow: hidden;
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(0.4) contrast(1.1);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
}

.slide:hover .media {
  filter: grayscale(0) contrast(1.3);
  transform: scale(1.1) translateX(10px);
}

/* --- High Velocity Controls --- */
.navBtn {
  position: absolute;
  z-index: 50;
  top: 50%;
  width: 50px;
  height: 70px;
  background: var(--c-accent);
  color: white;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: translateY(-50%) skewX(var(--skew-btn));
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 10px 0 20px rgba(0,0,0,0.3);
}

.navBtn:hover {
  background: white;
  color: #000;
  width: 65px;
}

.navBtn svg {
  transform: skewX(var(--skew-btn-rev));
}

.prevBtn { left: -10px; }
.nextBtn { right: -10px; }

/* --- Animated Speed Lines --- */
.speedLine {
  position: absolute;
  left: -150px;
  width: 80px;
  height: 1px;
  background: linear-gradient(90deg, transparent, white, transparent);
  opacity: 0.4;
  z-index: 10;
  pointer-events: none;
  animation: rush 0.8s linear infinite;
  animation-delay: var(--d);
}

@keyframes rush {
  0% { transform: translateX(0); opacity: 0; }
  20% { opacity: 0.7; }
  100% { transform: translateX(1200px); opacity: 0; }
}

.labelContainer {
  position: absolute;
  bottom: 30px;
  left: 0;
  z-index: 15;
}

.italicBadge {
  background: var(--c-accent);
  padding: 8px 30px;
  transform: skewX(-15deg);
  box-shadow: 10px 10px 0px rgba(0,0,0,0.3);
}

.fastText {
  display: block;
  color: white;
  font-family: sans-serif;
  font-style: italic;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: -1px;
  transform: skewX(15deg);
  font-size: 1.1rem;
}

.velocityBar {
  width: 0%;
  height: 4px;
  background: white;
  margin-top: 6px;
  transition: width 0.4s ease;
}

.slide:hover .velocityBar {
  width: 100%;
}

@media (max-width: 768px) {
  .slide { height: calc(var(--c-height) * 0.75); transform: skewX(-2deg); }
  .navBtn { display: none; }
}
</style>