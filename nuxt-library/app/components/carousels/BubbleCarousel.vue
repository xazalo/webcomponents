<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Atrás">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Siguiente">
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
          <div :class="$style.bubbleFrame">
            <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            
            <div v-if="item.label" :class="$style.pill">
              {{ item.label }}
            </div>

            <button :class="$style.actionBtn">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>
            </button>
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
  gap: '24px',
  color: '#4f46e5',
  radius: '60px',
  showScrollbar: false
});

const carouselRef = ref<HTMLElement | null>(null);
const isDragging = ref(false);
let startX: number, scrollLeft: number;

const scroll = (dir: 'next' | 'prev') => {
  if (!carouselRef.value) return;
  const scrollAmount = carouselRef.value.clientWidth * 0.7;
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
  '--c-radius': props.radius,
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  --bubble-bg: rgba(255, 255, 255, 0.75);
  --bubble-shadow: 0 20px 40px -10px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 40px 0;
  cursor: grab;
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
  height: 400px;
  scroll-snap-align: center;
}

/* --- Botones de Navegación Bubble --- */
.navBtn {
  position: absolute;
  z-index: 50;
  width: 52px;
  height: 52px;
  border-radius: 100px;
  background: white;
  border: none;
  color: #1a1a1a;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.navBtn:hover {
  background: var(--c-accent);
  color: white;
  transform: scale(1.15) translateY(-2px);
  box-shadow: 0 15px 30px rgba(0,0,0,0.15);
}

.prevBtn { left: 15px; }
.nextBtn { right: 15px; }

/* --- Bubble Frame --- */
.bubbleFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: white;
  border-radius: var(--c-radius);
  box-shadow: var(--bubble-shadow);
  overflow: hidden;
  border: 6px solid white;
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.slide:hover .bubbleFrame {
  transform: scale(1.03) rotate(1.5deg);
  border-color: var(--c-accent);
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 1.2s cubic-bezier(0.2, 1, 0.2, 1);
}

.slide:hover .media {
  transform: scale(1.15);
}

.pill {
  position: absolute;
  top: 25px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--bubble-bg);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  padding: 10px 20px;
  border-radius: 100px;
  font-size: 11px;
  font-weight: 800;
  color: #1a1a1a;
  border: 1px solid rgba(255, 255, 255, 0.5);
  white-space: nowrap;
  box-shadow: 0 4px 15px rgba(0,0,0,0.05);
  z-index: 10;
}

.actionBtn {
  position: absolute;
  bottom: 25px;
  right: 25px;
  width: 55px;
  height: 55px;
  border-radius: 100px;
  background: var(--c-accent);
  color: white;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  z-index: 10;
}

.actionBtn:hover {
  transform: scale(1.2) rotate(-15deg);
  box-shadow: 0 15px 30px rgba(0,0,0,0.2);
}

@media (max-width: 768px) {
  .slide { height: 320px; }
  .bubbleFrame { border-radius: 40px; }
  .navBtn { display: none; }
}
</style>