<template>
  <div 
    :class="[
      $style.bubbleCard, 
      $style[orientation], 
      { [$style.hasImage]: !!image }
    ]" 
    :style="cardStyles"
  >
    <div v-if="image" :class="$style.imageWrapper">
      <img :src="image" :alt="title" :class="$style.image" loading="lazy" />
      <div v-if="$slots.badge" :class="$style.imageBadge">
        <slot name="badge" />
      </div>
    </div>

    <div :class="$style.content">
      <header :class="$style.header">
        <span v-if="tag" :class="$style.tag">{{ tag }}</span>
        <h3 :class="$style.title">{{ title }}</h3>
        <p v-if="description" :class="$style.description">{{ description }}</p>
      </header>

      <div :class="$style.body">
        <slot />
      </div>

      <footer v-if="$slots.footer" :class="$style.footer">
        <slot name="footer" />
      </footer>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title: string;
  description?: string;
  tag?: string;
  image?: string;
  orientation?: 'vertical' | 'horizontal';
  align?: 'start' | 'center' | 'end';
  // Props Estilo Bubble
  primaryColor?: string;   // Color de acento (ej: rosa pastel)
  secondaryColor?: string; // Segundo color para gradientes
  bgCard?: string;
  textColor?: string;
  roundness?: string;      // Qué tan "pomposa" es
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  primaryColor: '#ff85a1',
  secondaryColor: '#ffb7c5',
  bgCard: '#ffffff',
  textColor: '#2d3436',
  roundness: '40px'
});

const cardStyles = computed(() => ({
  '--bb-primary': props.primaryColor,
  '--bb-secondary': props.secondaryColor,
  '--bb-bg': props.bgCard,
  '--bb-color': props.textColor,
  '--bb-radius': props.roundness,
  '--bb-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--bb-text-align': props.align,
  '--bb-shadow': `0 20px 40px -10px rgba(0, 0, 0, 0.08)`
}));
</script>

<style module>
.bubbleCard {
  background: var(--bb-bg);
  color: var(--bb-color);
  border-radius: var(--bb-radius);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100%;
  box-shadow: var(--bb-shadow);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); /* Efecto muelle */
  text-align: var(--bb-text-align);
  font-family: 'Quicksand', 'Nunito', sans-serif;
  border: 4px solid transparent;
}

.bubbleCard:hover {
  transform: scale(1.02) translateY(-5px);
  box-shadow: 0 30px 50px -15px rgba(0, 0, 0, 0.12);
  border-color: var(--bb-secondary);
}

/* --- ORIENTACIONES --- */
.vertical { flex-direction: column; }
.horizontal { flex-direction: row; }

@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
}

/* --- IMAGEN --- */
.imageWrapper {
  position: relative;
  overflow: hidden;
  margin: 12px;
  border-radius: calc(var(--bb-radius) * 0.8);
  flex-shrink: 0;
}

.vertical .imageWrapper {
  height: 220px;
}

.horizontal .imageWrapper {
  width: 40%;
  margin-right: 0;
}

@media (max-width: 768px) {
  .horizontal .imageWrapper {
    width: auto;
    margin-right: 12px;
  }
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.imageBadge {
  position: absolute;
  top: 12px;
  left: 12px;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 800;
  color: var(--bb-primary);
}

/* --- CONTENIDO --- */
.content {
  padding: 24px clamp(20px, 5vw, 32px);
  display: flex;
  flex-direction: column;
  align-items: var(--bb-align);
  justify-content: center;
  gap: 8px;
}

.tag {
  color: var(--bb-primary);
  background: color-mix(in srgb, var(--bb-primary), transparent 85%);
  padding: 4px 14px;
  border-radius: 50px;
  font-size: 0.75rem;
  font-weight: 800;
  margin-bottom: 4px;
}

.title {
  margin: 0;
  font-size: 1.6rem;
  font-weight: 800;
  letter-spacing: -0.5px;
}

.description {
  margin: 0;
  font-size: 0.95rem;
  line-height: 1.4;
  opacity: 0.7;
  font-weight: 600;
}

.body {
  width: 100%;
}

.footer {
  margin-top: auto;
  padding-top: 20px;
  width: 100%;
}
</style>