<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <div :class="$style.controls">
      <button @click="scroll('prev')" :class="$style.navBtn" aria-label="Prev">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="m15 18-6-6 6-6"/></svg>
      </button>
      <button @click="scroll('next')" :class="$style.navBtn" aria-label="Next">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="m9 18 6-6 6-6"/></svg>
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
          <div :class="$style.mediaContainer">
            <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            
            <div v-if="item.label" :class="$style.caption">
              <span :class="$style.labelTag">{{ item.label }}</span>
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
  aspectRatio?: string; // Opcional por item
  width?: string;       // Opcional por item
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
  defaultSlideWidth: 'auto', // Permite que el aspecto mande sobre el ancho
  gap: '16px',
  color: '#4f46e5',
  radius: '16px',
  showScrollbar: false
});

// Lógica de Scroll y Drag (se mantiene igual que la anterior)
const carouselRef = ref<HTMLElement | null>(null);
const isDragging = ref(false);
let startX: number, scrollLeft: number;

const scroll = (dir: 'next' | 'prev') => {
  if (!carouselRef.value) return;
  carouselRef.value.scrollBy({ 
    left: dir === 'next' ? 400 : -400, 
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
  --bento-bg: #ffffff;
  --bento-border: rgba(0, 0, 0, 0.04);
  --bento-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.08);
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  scrollbar-width: none; /* Ocultamos por defecto para limpieza bento */
  cursor: grab;
  padding: 10px 0;
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
  align-items: center;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: 380px; /* Altura constante para coherencia visual */
  scroll-snap-align: start;
  user-select: none;
}

.mediaContainer {
  position: relative;
  width: 100%;
  height: 100%;
  background: #f8fafc;
  border-radius: var(--c-radius);
  border: 1px solid var(--bento-border);
  box-shadow: var(--bento-shadow);
  overflow: hidden;
  transition: transform 0.4s cubic-bezier(0.2, 0, 0, 1);
}

.slide:hover .mediaContainer {
  transform: translateY(-4px); /* Sutil elevación */
  box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.12);
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: scale 0.6s ease;
}

.slide:hover .media {
  scale: 1.05;
}

/* Caption: Menos degradado, más "Floating Badge" */
.caption {
  position: absolute;
  top: 16px;
  left: 16px;
  pointer-events: none;
}

.labelTag {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  padding: 6px 12px;
  border-radius: 10px;
  font-family: 'Inter', ui-monospace, SFMono-Regular, monospace;
  font-size: 10px;
  font-weight: 700;
  color: #1a1a1a;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  border: 1px solid rgba(255, 255, 255, 0.5);
  box-shadow: 0 4px 10px rgba(0,0,0,0.05);
}

/* Controles: Agrupados en una esquina o arriba para estilo Bento */
.controls {
  display: flex;
  gap: 8px;
  position: absolute;
  top: -60px; /* Fuera del carousel, estilo header */
  right: 0;
}

.navBtn {
  width: 36px;
  height: 36px;
  border-radius: 12px;
  background: white;
  border: 1px solid var(--bento-border);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: #64748b;
  transition: all 0.2s ease;
  box-shadow: 0 2px 5px rgba(0,0,0,0.02);
}

.navBtn:hover {
  background: var(--c-accent);
  color: white;
  border-color: var(--c-accent);
}

@media (max-width: 768px) {
  .controls { display: none; }
  .slide { height: 300px; }
}
</style>