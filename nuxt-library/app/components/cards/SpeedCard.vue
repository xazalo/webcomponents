<template>
  <div 
    :class="[
      $style.speedCard, 
      $style[orientation], 
      { [$style.hasImage]: !!image }
    ]" 
    :style="cardStyles"
  >
    <div :class="$style.topBar"></div>
    <div :class="$style.speedLines"></div>

    <div v-if="image" :class="$style.imageWrapper">
      <img :src="image" :alt="title" :class="$style.image" loading="lazy" />
      <div v-if="$slots.badge" :class="$style.imageBadge">
        <slot name="badge" />
      </div>
      <div :class="$style.imageOverlay"></div>
    </div>

    <div :class="$style.content">
      <header :class="$style.header">
        <div :class="$style.meta">
          <span v-if="tag" :class="$style.tag">{{ tag }}</span>
          <span :class="$style.rpmIndicator">REV_LIMIT: 9.5k</span>
        </div>
        <h3 :class="$style.title">{{ title }}</h3>
        <p v-if="description" :class="$style.description">{{ description }}</p>
      </header>

      <div :class="$style.body">
        <slot />
      </div>

      <footer v-if="$slots.footer" :class="$style.footer">
        <div :class="$style.footerAccent"></div>
        <slot name="footer" />
      </footer>
    </div>

    <div :class="$style.cornerDetail"></div>
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
  // Props Speed
  accentColor?: string; // Color "Nitro" (Rojo, Naranja, Amarillo)
  bgCard?: string;
  skewAmount?: string;   // Grados de inclinación
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  accentColor: '#fcee0a', // Amarillo Racing
  bgCard: '#121212',
  skewAmount: '-6deg'
});

const cardStyles = computed(() => ({
  '--speed-accent': props.accentColor,
  '--speed-bg': props.bgCard,
  '--speed-skew': props.skewAmount,
  '--speed-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--speed-text-align': props.align,
}));
</script>

<style module>
.speedCard {
  background: var(--speed-bg);
  color: #fff;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  height: 100%;
  border-left: 4px solid var(--speed-accent);
  transition: all 0.3s cubic-bezier(0.2, 1, 0.3, 1);
  font-family: 'Exo 2', 'Orbitron', sans-serif;
  text-align: var(--speed-text-align);
}

.speedCard:hover {
  transform: skewX(-1deg) scale(1.02);
  background: #1a1a1a;
  box-shadow: -10px 10px 0px color-mix(in srgb, var(--speed-accent), transparent 80%);
}

/* --- ELEMENTOS DINÁMICOS --- */
.topBar {
  height: 2px;
  width: 100%;
  background: linear-gradient(90deg, var(--speed-accent), transparent);
}

.speedLines {
  position: absolute;
  top: 0; right: 0;
  width: 100px; height: 100%;
  background: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 10px,
    rgba(255, 255, 255, 0.03) 10px,
    rgba(255, 255, 255, 0.03) 11px
  );
}

/* --- MEDIA --- */
.imageWrapper {
  position: relative;
  clip-path: polygon(0 0, 100% 0, 100% 90%, 0% 100%);
  flex-shrink: 0;
}

.horizontal .imageWrapper {
  width: 45%;
  clip-path: polygon(0 0, 90% 0, 100% 100%, 0% 100%);
}

.image {
  width: 100%;
  height: 220px;
  object-fit: cover;
  filter: grayscale(0.4) contrast(1.2);
  transition: filter 0.3s, transform 0.5s;
}

.speedCard:hover .image {
  filter: grayscale(0) contrast(1.1);
  transform: scale(1.1) rotate(-1deg);
}

.imageOverlay {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to top, var(--speed-bg), transparent);
}

.imageBadge {
  position: absolute;
  top: 15px; left: 15px;
  background: var(--speed-accent);
  color: #000;
  font-weight: 900;
  font-style: italic;
  padding: 3px 10px;
  font-size: 0.7rem;
  transform: skewX(var(--speed-skew));
}

/* --- CONTENIDO --- */
.content {
  padding: 30px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  flex: 1;
  z-index: 2;
}

.meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tag {
  color: var(--speed-accent);
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  font-size: 0.75rem;
  letter-spacing: 1px;
}

.rpmIndicator {
  font-size: 0.6rem;
  color: rgba(255, 255, 255, 0.3);
  font-weight: 700;
}

.title {
  margin: 0;
  font-size: 1.8rem;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  letter-spacing: -1px;
  line-height: 0.9;
}

.description {
  margin: 0;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.4;
  font-weight: 500;
  max-width: 90%;
}

.footerAccent {
  width: 40px;
  height: 4px;
  background: var(--speed-accent);
  margin-bottom: 15px;
}

.cornerDetail {
  position: absolute;
  bottom: -10px;
  right: -10px;
  width: 30px;
  height: 30px;
  background: var(--speed-accent);
  transform: rotate(45deg);
  opacity: 0.5;
}

@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
  .horizontal .imageWrapper { width: 100%; clip-path: polygon(0 0, 100% 0, 100% 90%, 0% 100%); }
}
</style>