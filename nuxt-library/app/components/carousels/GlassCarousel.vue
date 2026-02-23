<template>
  <div :class="$style.wrapper" :style="carouselStyles">
    <button v-if="showControls" @click="scroll('prev')" :class="[$style.navBtn, $style.prevBtn]" aria-label="Atrás">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
    </button>
    
    <button v-if="showControls" @click="scroll('next')" :class="[$style.navBtn, $style.nextBtn]" aria-label="Adelante">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
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
          <div :class="$style.glassFrame" @click="$emit('item-click', item)">
            <div v-if="showReflection" :class="$style.reflection" />
            
            <div :class="$style.mediaWrapper">
              <MediaRender :url="item.url" :alt="item.alt" :class="$style.media" />
            </div>
            
            <div v-if="item.label" :class="$style.glassCaption">
              <div :class="$style.blurOverlay" />
              <span :class="$style.labelText">{{ item.label }}</span>
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
  borderRadius?: string;
  cardHeight?: string;
  blurStrength?: string;     // e.g., '12px'
  glassOpacity?: number;      // 0 to 1
  shadowColor?: string;      // rgba value
  showControls?: boolean;
  showReflection?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  defaultAspectRatio: '1/1',
  gap: '30px',
  accentColor: 'rgba(255, 255, 255, 0.5)',
  borderRadius: '24px',
  cardHeight: '400px',
  blurStrength: '12px',
  glassOpacity: 0.15,
  shadowColor: 'rgba(31, 38, 135, 0.15)',
  showControls: true,
  showReflection: true
});

defineEmits(['item-click']);

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
  '--c-gap': props.gap,
  '--c-radius': props.borderRadius,
  '--c-height': props.cardHeight,
  '--glass-blur': props.blurStrength,
  '--glass-opacity': props.glassOpacity,
  '--glass-shadow-color': props.shadowColor,
}));
</script>

<style module>
.wrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.carouselRoot {
  width: 100%;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  padding: 40px 0;
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

/* --- Glass Controls --- */
.navBtn {
  position: absolute;
  z-index: 50;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.navBtn:hover {
  background: rgba(255, 255, 255, 0.25);
  border-color: rgba(255, 255, 255, 0.5);
  transform: translateY(-50%) scale(1.1);
}

.prevBtn { left: 20px; top: 50%; transform: translateY(-50%); }
.nextBtn { right: 20px; top: 50%; transform: translateY(-50%); }

/* --- Glass Frame --- */
.glassFrame {
  position: relative;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, var(--glass-opacity));
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  border-radius: var(--c-radius);
  border: 1px solid rgba(255, 255, 255, calc(var(--glass-opacity) + 0.1));
  box-shadow: 0 8px 32px 0 var(--glass-shadow-color);
  padding: 12px;
  overflow: hidden;
  transition: transform 0.4s cubic-bezier(0.165, 0.84, 0.44, 1), border-color 0.4s ease;
}

.slide:hover .glassFrame {
  transform: translateY(-10px);
  border-color: var(--c-accent);
}

.reflection {
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 50%;
  background: linear-gradient(
    rgba(255, 255, 255, 0.2) 0%, 
    rgba(255, 255, 255, 0) 100%
  );
  pointer-events: none;
  z-index: 2;
}

.mediaWrapper {
  width: 100%;
  height: 100%;
  border-radius: calc(var(--c-radius) - 8px);
  overflow: hidden;
}

.media {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s cubic-bezier(0.2, 1, 0.3, 1);
}

.slide:hover .media {
  transform: scale(1.1);
}

.glassCaption {
  position: absolute;
  bottom: 24px;
  left: 24px;
  right: 24px;
  padding: 12px 20px;
  border-radius: 14px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.2);
  z-index: 5;
}

.blurOverlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(calc(var(--glass-blur) * 1.5));
  z-index: 1;
}

.labelText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  text-shadow: 0 2px 10px rgba(0,0,0,0.3);
}

@media (max-width: 768px) {
  .slide { height: calc(var(--c-height) * 0.85); }
  .navBtn { display: none; }
}
</style>