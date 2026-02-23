<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button v-if="showControls" @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Atrás">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button v-if="showControls" @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Adelante">
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
          <div :class="$style.neuFrame" @click="$emit('select', item)">
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

interface Props {
  items: CarouselItem[];
  defaultAspectRatio?: string;
  gap?: string;
  baseColor?: string;     // The "Surface" color
  lightSource?: string;   // The highlight color
  shadowSource?: string;  // The dark shadow color
  accentColor?: string;   // For hover states
  borderRadius?: string;
  cardHeight?: string;
  depth?: number;         // Scale of the shadows (1-10)
  showControls?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  defaultAspectRatio: '1/1',
  gap: '35px',
  baseColor: '#e0e5ec',
  lightSource: '#ffffff',
  shadowSource: '#a3b1c6',
  accentColor: '#4f46e5',
  borderRadius: '40px',
  cardHeight: '400px',
  depth: 6,
  showControls: true
});

defineEmits(['select']);

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
  '--neu-base': props.baseColor,
  '--neu-light': props.lightSource,
  '--neu-shadow': props.shadowSource,
  '--c-accent': props.accentColor,
  '--c-gap': props.gap,
  '--c-radius': props.borderRadius,
  '--c-height': props.cardHeight,
  '--d-outer': `${props.depth * 1.5}px`,
  '--d-inner': `${props.depth}px`,
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  background: var(--neu-base);
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
  cursor: grab;
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
  height: var(--c-height);
  scroll-snap-align: center;
}

/* --- Neumorphic Controls --- */
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
  box-shadow: var(--d-inner) var(--d-inner) calc(var(--d-inner) * 2) var(--neu-shadow), 
              calc(var(--d-inner) * -1) calc(var(--d-inner) * -1) calc(var(--d-inner) * 2) var(--neu-light);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.navBtn:hover {
  color: var(--c-accent);
}

.navBtn:active {
  box-shadow: inset 4px 4px 8px var(--neu-shadow), 
              inset -4px -4px 8px var(--neu-light);
  transform: scale(0.96);
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
  box-shadow: var(--d-outer) var(--d-outer) calc(var(--d-outer) * 2) var(--neu-shadow), 
              calc(var(--d-outer) * -1) calc(var(--d-outer) * -1) calc(var(--d-outer) * 2) var(--neu-light);
  transition: all 0.4s ease;
  cursor: pointer;
}

.slide:hover .neuFrame {
  transform: translateY(-5px);
  box-shadow: calc(var(--d-outer) * 1.3) calc(var(--d-outer) * 1.3) calc(var(--d-outer) * 2.5) var(--neu-shadow), 
              calc(var(--d-outer) * -1.3) calc(var(--d-outer) * -1.3) calc(var(--d-outer) * 2.5) var(--neu-light);
}

.mediaInset {
  width: 100%;
  height: 100%;
  border-radius: calc(var(--c-radius) * 0.6);
  overflow: hidden;
  box-shadow: inset var(--d-inner) var(--d-inner) calc(var(--d-inner) * 2) var(--neu-shadow), 
              inset calc(var(--d-inner) * -1) calc(var(--d-inner) * -1) calc(var(--d-inner) * 2) var(--neu-light);
  background: var(--neu-base);
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.95;
  mix-blend-mode: multiply; /* Helps the image sit "into" the background */
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
  border-radius: calc(var(--c-radius) * 0.4);
  box-shadow: 4px 4px 8px var(--neu-shadow), 
              -4px -4px 8px var(--neu-light);
  transition: color 0.3s ease;
}

.slide:hover .neuLabel {
  color: var(--c-accent);
}

@media (max-width: 768px) {
  .slide { height: calc(var(--c-height) * 0.8); }
  .navBtn { display: none; }
}
</style>