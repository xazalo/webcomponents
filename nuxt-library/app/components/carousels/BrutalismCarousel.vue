<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <div :class="$style.controls">
      <button @click="scroll('prev')" :class="$style.navBtn" aria-label="Anterior">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4"><path d="m15 18-6-6 6-6"/></svg>
      </button>
      <button @click="scroll('next')" :class="$style.navBtn" aria-label="Siguiente">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4"><path d="m9 18 6-6-6-6"/></svg>
      </button>
    </div>
    
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
          <div :class="$style.brutalFrame">
            <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            
            <div v-if="item.label" :class="$style.sticker">
              {{ item.label }}
            </div>
            
            <div :class="$style.edgeDecorator" />
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
  width?: string;
}

const props = withDefaults(defineProps<{
  items: CarouselItem[];
  defaultAspectRatio?: string;
  defaultSlideWidth?: string;
  gap?: string;
  color?: string;
  radius?: string;
  showScrollbar?: boolean;
}>(), {
  defaultAspectRatio: '1/1',
  defaultSlideWidth: 'auto',
  gap: '30px',
  color: '#A3E635', // Un lima brutalista típico
  radius: '0px',
  showScrollbar: true
});

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
  '--c-accent': props.color,
  '--c-gap': props.gap,
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  padding: 20px;
  background: #fff; /* El brutalismo suele ir sobre blanco o colores muy vibrantes */
}

.controls {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
}

.navBtn {
  width: 60px;
  height: 60px;
  background: #fff;
  border: 4px solid #000;
  box-shadow: 6px 6px 0px #000;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.1s;
}

.navBtn:hover {
  background: var(--c-accent);
  transform: translate(-2px, -2px);
  box-shadow: 8px 8px 0px #000;
}

.navBtn:active {
  transform: translate(6px, 6px);
  box-shadow: 0px 0px 0px #000;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding-bottom: 30px;
  cursor: crosshair;
}

/* Scrollbar ruda */
.carouselRoot::-webkit-scrollbar { height: 16px; }
.carouselRoot::-webkit-scrollbar-track { background: #fff; border: 4px solid #000; }
.carouselRoot::-webkit-scrollbar-thumb { background: #000; border: 2px solid #fff; }

.isDragging {
  scroll-snap-type: none;
  cursor: grabbing;
}

.track {
  display: flex;
  gap: var(--c-gap);
  width: max-content;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: 420px;
  scroll-snap-align: start;
}

.brutalFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: #000;
  border: 4px solid #000;
  box-shadow: 10px 10px 0px 0px #000;
  transition: all 0.15s cubic-bezier(0, 0, 0, 1.25);
  overflow: hidden;
}

.slide:hover .brutalFrame {
  transform: translate(-4px, -4px);
  box-shadow: 14px 14px 0px 0px var(--c-accent);
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(0.2) contrast(1.1);
  transition: filter 0.3s ease;
}

.slide:hover .media {
  filter: grayscale(0) contrast(1.2);
}

.sticker {
  position: absolute;
  top: 20px;
  right: -10px;
  background: var(--c-accent);
  color: #000;
  padding: 10px 20px;
  font-family: 'Arial Black', sans-serif;
  font-weight: 900;
  font-size: 1.2rem;
  text-transform: uppercase;
  border: 4px solid #000;
  transform: rotate(3deg);
  z-index: 5;
  box-shadow: 5px 5px 0px #000;
}

.edgeDecorator {
  position: absolute;
  bottom: 15px;
  left: 15px;
  width: 30px;
  height: 30px;
  background: #fff;
  border: 4px solid #000;
  border-radius: 50%;
  z-index: 4;
}

@media (max-width: 768px) {
  .slide { height: 320px; }
  .navBtn { width: 50px; height: 50px; }
}
</style>