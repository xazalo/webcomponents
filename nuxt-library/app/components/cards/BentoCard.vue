<template>
  <div 
    :class="[
      $style.bentoCard, 
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
  /** 'vertical' o 'horizontal' */
  orientation?: 'vertical' | 'horizontal';
  /** 'start', 'center', 'end' */
  align?: 'start' | 'center' | 'end';
  // Personalización por Props
  accentColor?: string;
  bgCard?: string;
  textColor?: string;
  aspectRatio?: string;
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  accentColor: '#000',
  bgCard: '#ffffff',
  textColor: '#1d1d1f',
  aspectRatio: 'auto'
});

const cardStyles = computed(() => ({
  '--bento-accent': props.accentColor,
  '--bento-bg': props.bgCard,
  '--bento-color': props.textColor,
  '--bento-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--bento-aspect': props.aspectRatio,
  '--bento-text-align': props.align
}));
</script>

<style module>
.bentoCard {
  background: var(--bento-bg);
  color: var(--bento-color);
  border-radius: 24px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(0,0,0,0.05);
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1), box-shadow 0.3s;
  height: 100%;
  aspect-ratio: var(--bento-aspect);
  text-align: var(--bento-text-align);
}

.bentoCard:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.08);
}

/* --- ORIENTACIONES --- */

/* Vertical (Imagen arriba, texto abajo) */
.vertical {
  flex-direction: column;
}

/* Horizontal (Imagen izquierda, texto derecha) */
.horizontal {
  flex-direction: row;
}

@media (max-width: 640px) {
  .horizontal {
    flex-direction: column; /* Colapsa en móvil para no romper el grid */
  }
}

/* --- IMAGEN --- */
.imageWrapper {
  position: relative;
  overflow: hidden;
  flex-shrink: 0;
  background: #f2f2f2;
}

.vertical .imageWrapper {
  width: 100%;
  height: 200px; /* Altura base para vertical */
}

.horizontal .imageWrapper {
  width: 40%; /* Proporción para horizontal */
  min-height: 100%;
}

@media (max-width: 640px) {
  .horizontal .imageWrapper {
    width: 100%;
    height: 180px;
  }
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.bentoCard:hover .image {
  transform: scale(1.05);
}

.imageBadge {
  position: absolute;
  top: 15px;
  left: 15px;
  z-index: 2;
}

/* --- CONTENIDO --- */
.content {
  padding: clamp(16px, 4vw, 24px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: var(--bento-align);
  flex-grow: 1;
  gap: 12px;
}

.tag {
  display: inline-block;
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: var(--bento-accent);
  margin-bottom: 4px;
}

.title {
  margin: 0;
  font-size: clamp(1.2rem, 3vw, 1.5rem);
  font-weight: 700;
  line-height: 1.2;
}

.description {
  margin: 0;
  font-size: 0.95rem;
  line-height: 1.5;
  opacity: 0.7;
  font-weight: 500;
}

.body {
  width: 100%;
  margin-top: 5px;
}

.footer {
  margin-top: auto;
  padding-top: 15px;
  width: 100%;
}
</style>