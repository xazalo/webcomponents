<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Atrás">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Adelante">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
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
          <div :class="$style.neuFrame">
            <div :class="$style.mediaInset">
              <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            </div>
            
            <div v-if="item.label" :class="$style.neuLabel">
              {{ item.label }}
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
  gap: '35px',
  color: '#e0e5ec',
  radius: '40px',
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
  --neu-base: #e0e5ec; 
  --neu-light: #ffffff;
  --neu-shadow: #a3b1c6;
  display: flex;
  align-items: center;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 50px 0;
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
  padding: 0 80px;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: 400px;
  scroll-snap-align: center;
}

/* --- Botones de Navegación Neumórficos --- */
.navBtn {
  position: absolute;
  z-index: 10;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: var(--neu-base);
  border: none;
  color: #748194;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 6px 6px 12px var(--neu-shadow), 
              -6px -6px 12px var(--neu-light);
  transition: all 0.2s ease;
}

.navBtn:hover {
  color: #4f46e5;
}

.navBtn:active {
  box-shadow: inset 4px 4px 8px var(--neu-shadow), 
              inset -4px -4px 8px var(--neu-light);
  transform: scale(0.95);
}

.prevBtn { left: 15px; }
.nextBtn { right: 15px; }

/* --- Card Frame --- */
.neuFrame {
  width: 100%;
  height: 100%;
  background: var(--neu-base);
  border-radius: var(--c-radius);
  padding: 20px;
  display: flex;
  flex-direction: column;
  box-shadow: 9px 9px 16px var(--neu-shadow), 
              -9px -9px 16px var(--neu-light);
  transition: all 0.3s ease;
}

.slide:hover .neuFrame {
  box-shadow: 12px 12px 20px var(--neu-shadow), 
              -12px -12px 20px var(--neu-light);
}

.mediaInset {
  width: 100%;
  height: 100%;
  border-radius: 25px;
  overflow: hidden;
  box-shadow: inset 6px 6px 12px var(--neu-shadow), 
              inset -6px -6px 12px var(--neu-light);
  background: var(--neu-base);
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.9;
  mix-blend-mode: multiply; 
}

.neuLabel {
  margin-top: 20px;
  padding: 10px 0;
  color: #748194;
  font-size: 0.75rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1px;
  text-align: center;
  background: var(--neu-base);
  border-radius: 15px;
  box-shadow: 4px 4px 8px var(--neu-shadow), 
              -4px -4px 8px var(--neu-light);
}

@media (max-width: 768px) {
  .slide { height: 320px; }
  .navBtn { display: none; }
}
</style>