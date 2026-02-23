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
  orientation?: 'vertical' | 'horizontal';
  align?: 'start' | 'center' | 'end';
  // Props Estilo Brutalista
  primaryColor?: string; // El color del borde y sombra
  accentColor?: string;  // El color del Tag y detalles
  bgCard?: string;
  textColor?: string;
  borderWidth?: string;
  shadowDepth?: string;
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  primaryColor: '#000000',
  accentColor: '#f4fd05', // Amarillo neón típico
  bgCard: '#ffffff',
  textColor: '#000000',
  borderWidth: '4px',
  shadowDepth: '8px'
});

const cardStyles = computed(() => ({
  '--neo-border-color': props.primaryColor,
  '--neo-accent': props.accentColor,
  '--neo-bg': props.bgCard,
  '--neo-color': props.textColor,
  '--neo-border-w': props.borderWidth,
  '--neo-shadow-d': props.shadowDepth,
  '--neo-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--neo-text-align': props.align
}));
</script>

<style module>
.bentoCard {
  background: var(--neo-bg);
  color: var(--neo-color);
  border: var(--neo-border-w) solid var(--neo-border-color);
  box-shadow: var(--neo-shadow-d) var(--neo-shadow-d) 0px var(--neo-border-color);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100%;
  transition: transform 0.1s ease, box-shadow 0.1s ease;
  text-align: var(--neo-text-align);
  font-family: 'Arial Black', sans-serif; /* Tipografía pesada */
}

.bentoCard:hover {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--neo-shadow-d) + 2px) calc(var(--neo-shadow-d) + 2px) 0px var(--neo-border-color);
}

.bentoCard:active {
  transform: translate(var(--neo-shadow-d), var(--neo-shadow-d));
  box-shadow: 0px 0px 0px var(--neo-border-color);
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
  flex-shrink: 0;
  border-bottom: var(--neo-border-w) solid var(--neo-border-color);
}

.horizontal .imageWrapper {
  width: 45%;
  border-bottom: none;
  border-right: var(--neo-border-w) solid var(--neo-border-color);
}

@media (max-width: 768px) {
  .horizontal .imageWrapper {
    width: 100%;
    border-right: none;
    border-bottom: var(--neo-border-w) solid var(--neo-border-color);
  }
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(0.2) contrast(1.1); /* Toque industrial */
}

.imageBadge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: var(--neo-accent);
  border: 2px solid var(--neo-border-color);
  padding: 4px 8px;
  font-size: 0.7rem;
  font-weight: 900;
  z-index: 2;
}

/* --- CONTENIDO --- */
.content {
  padding: 24px;
  display: flex;
  flex-direction: column;
  align-items: var(--neo-align);
  justify-content: center;
  gap: 12px;
}

.tag {
  background: var(--neo-accent);
  color: var(--neo-border-color);
  border: 2px solid var(--neo-border-color);
  padding: 2px 10px;
  font-size: 0.7rem;
  font-weight: 900;
  text-transform: uppercase;
  width: fit-content;
}

.title {
  margin: 0;
  font-size: 1.8rem;
  font-weight: 900;
  line-height: 1;
  text-transform: uppercase;
}

.description {
  margin: 0;
  font-size: 1rem;
  line-height: 1.3;
  font-weight: 700;
}

.footer {
  margin-top: auto;
  padding-top: 15px;
  width: 100%;
}
</style>