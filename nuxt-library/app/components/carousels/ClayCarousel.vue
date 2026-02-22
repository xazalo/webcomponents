<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Atrás">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Siguiente">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
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
          <div :class="$style.clayFrame">
            <div :class="$style.mediaInsetText">
              <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            </div>
            
            <div v-if="item.label" :class="$style.clayLabel">
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
  gap: '40px',
  color: '#4f46e5',
  radius: '48px',
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
  --clay-bg: #ffffff;
  display: flex;
  align-items: center;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 50px 0;
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
  padding: 0 80px;
}

.slide {
  flex: 0 0 auto;
  aspect-ratio: var(--slide-aspect);
  height: 420px;
  scroll-snap-align: center;
}

/* --- Botones de Navegación Clay --- */
.navBtn {
  position: absolute;
  z-index: 20;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--clay-bg);
  border: none;
  color: var(--c-accent);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  /* El look 3D del botón */
  box-shadow: 8px 8px 16px rgba(0, 0, 0, 0.1), 
              inset -6px -6px 12px rgba(0, 0, 0, 0.05), 
              inset 6px 6px 12px rgba(255, 255, 255, 0.8);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.navBtn:hover {
  transform: scale(1.1);
  color: #000;
}

.navBtn:active {
  transform: scale(0.9);
  box-shadow: 4px 4px 8px rgba(0, 0, 0, 0.1), 
              inset -10px -10px 20px rgba(0, 0, 0, 0.05), 
              inset 10px 10px 20px rgba(255, 255, 255, 0.8);
}

.prevBtn { left: 20px; }
.nextBtn { right: 20px; }

/* --- Clay Frame --- */
.clayFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: var(--clay-bg);
  border-radius: var(--c-radius);
  box-shadow: 16px 16px 32px rgba(0, 0, 0, 0.08), 
              inset -12px -12px 24px rgba(0, 0, 0, 0.05), 
              inset 12px 12px 24px rgba(255, 255, 255, 0.9);
  padding: 20px;
  display: flex;
  flex-direction: column;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.slide:hover .clayFrame {
  transform: translateY(-10px) rotate(1deg);
  box-shadow: 25px 35px 50px rgba(0, 0, 0, 0.1), 
              inset -8px -8px 16px rgba(0, 0, 0, 0.05), 
              inset 8px 8px 16px rgba(255, 255, 255, 0.9);
}

.mediaInsetText {
  width: 100%;
  height: 100%;
  border-radius: calc(var(--c-radius) * 0.7);
  overflow: hidden;
  box-shadow: inset 6px 6px 15px rgba(0,0,0,0.1);
  background: #f8fafc;
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s ease;
}

.slide:hover .media {
  transform: scale(1.05);
}

.clayLabel {
  margin-top: 18px;
  background: var(--c-accent);
  color: white;
  padding: 10px 24px;
  border-radius: 25px;
  font-weight: 800;
  font-size: 0.85rem;
  text-align: center;
  box-shadow: inset -4px -4px 10px rgba(0, 0, 0, 0.2), 
              inset 4px 4px 10px rgba(255, 255, 255, 0.3),
              6px 10px 20px rgba(0,0,0,0.1);
  align-self: center;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

@media (max-width: 768px) {
  .slide { height: 350px; }
  .navBtn { display: none; }
}
</style>