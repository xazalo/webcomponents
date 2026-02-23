<template>
  <div 
    :class="[
      $style.clayCard, 
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
  // Props Estilo Clay
  clayColor?: string;     // Color base (ej: azul cielo)
  textColor?: string;
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  clayColor: '#ffffff',
  textColor: '#4b5563',
  borderRadius: '32px'
});

const cardStyles = computed(() => ({
  '--clay-bg': props.clayColor,
  '--clay-color': props.textColor,
  '--clay-radius': props.borderRadius,
  '--clay-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--clay-text-align': props.align,
  // El secreto del Claymorphism: Sombras externas e internas
  '--clay-shadow-outer': `20px 20px 40px rgba(0, 0, 0, 0.12)`,
  '--clay-shadow-inner': `inset -8px -8px 12px rgba(0, 0, 0, 0.1), inset 8px 8px 12px rgba(255, 255, 255, 0.8)`
}));
</script>

<style module>
.clayCard {
  background: var(--clay-bg);
  color: var(--clay-color);
  border-radius: var(--clay-radius);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100%;
  box-shadow: var(--clay-shadow-outer), var(--clay-shadow-inner);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  text-align: var(--clay-text-align);
  font-family: 'Inter', system-ui, sans-serif;
  border: 1px solid rgba(255, 255, 255, 0.4); /* Brillo sutil en el borde */
}

.clayCard:hover {
  transform: translateY(-8px) scale(1.01);
  box-shadow: 25px 25px 50px rgba(0, 0, 0, 0.15), var(--clay-shadow-inner);
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
  margin: 16px;
  border-radius: calc(var(--clay-radius) * 0.7);
  box-shadow: inset 4px 4px 8px rgba(0,0,0,0.1);
  flex-shrink: 0;
}

.vertical .imageWrapper { height: 200px; }
.horizontal .imageWrapper { width: 40%; margin-right: 0; }

@media (max-width: 768px) {
  .horizontal .imageWrapper { width: auto; margin-right: 16px; }
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  /* El Clay suele suavizar un poco las imágenes */
  opacity: 0.95;
}

.imageBadge {
  position: absolute;
  top: 12px;
  left: 12px;
  background: white;
  padding: 6px 14px;
  border-radius: 15px;
  font-size: 0.7rem;
  font-weight: 800;
  box-shadow: 4px 4px 8px rgba(0,0,0,0.05);
}

/* --- CONTENIDO --- */
.content {
  padding: 32px;
  display: flex;
  flex-direction: column;
  align-items: var(--clay-align);
  justify-content: center;
  gap: 10px;
}

.tag {
  font-size: 0.7rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  opacity: 0.5;
}

.title {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 800;
  color: #1f2937;
  letter-spacing: -0.5px;
}

.description {
  margin: 0;
  font-size: 0.95rem;
  line-height: 1.6;
  opacity: 0.8;
}

.footer {
  margin-top: auto;
  padding-top: 20px;
  width: 100%;
}
</style>