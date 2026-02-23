<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <div v-if="showControls" :class="$style.controls">
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
  aspectRatio?: string;
}

interface Props {
  items: CarouselItem[];
  defaultAspectRatio?: string;
  gap?: string;
  accentColor?: string;
  cardRadius?: string;
  cardHeight?: string;
  cardBg?: string;
  borderColor?: string;
  showControls?: boolean;
  labelBg?: string;
  labelTextColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  defaultAspectRatio: '1/1',
  gap: '16px',
  accentColor: '#4f46e5',
  cardRadius: '16px',
  cardHeight: '380px',
  cardBg: '#f8fafc',
  borderColor: 'rgba(0, 0, 0, 0.04)',
  showControls: true,
  labelBg: 'rgba(255, 255, 255, 0.85)',
  labelTextColor: '#1a1a1a'
});

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
  '--c-accent': props.accentColor,
  '--c-gap': props.gap,
  '--c-radius': props.cardRadius,
  '--card-height': props.cardHeight,
  '--card-bg': props.cardBg,
  '--card-border': props.borderColor,
  '--label-bg': props.labelBg,
  '--label-text': props.labelTextColor
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
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
  height: var(--card-height);
  scroll-snap-align: start;
  user-select: none;
}

.mediaContainer {
  position: relative;
  width: 100%;
  height: 100%;
  background: var(--card-bg);
  border-radius: var(--c-radius);
  border: 1px solid var(--card-border);
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  transition: transform 0.4s cubic-bezier(0.2, 0, 0, 1), box-shadow 0.4s ease;
}

.slide:hover .mediaContainer {
  transform: translateY(-4px);
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

.caption {
  position: absolute;
  top: 16px;
  left: 16px;
  pointer-events: none;
}

.labelTag {
  background: var(--label-bg);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  padding: 6px 12px;
  border-radius: 10px;
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 10px;
  font-weight: 700;
  color: var(--label-text);
  letter-spacing: 0.05em;
  text-transform: uppercase;
  border: 1px solid rgba(255, 255, 255, 0.5);
}

.controls {
  display: flex;
  gap: 8px;
  position: absolute;
  top: -50px;
  right: 0;
}

.navBtn {
  width: 36px;
  height: 36px;
  border-radius: 12px;
  background: white;
  border: 1px solid var(--card-border);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: #64748b;
  transition: all 0.2s ease;
}

.navBtn:hover {
  background: var(--c-accent);
  color: white;
  border-color: var(--c-accent);
}

@media (max-width: 768px) {
  .controls { display: none; }
  .slide { height: calc(var(--card-height) * 0.8); }
}
</style>