<template>
  <div 
    :class="[
      $style.glassCard, 
      $style[orientation], 
      { [$style.hasImage]: !!image }
    ]" 
    :style="cardStyles"
  >
    <div :class="$style.glare"></div>

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
  // Props Glassmorphism
  blurAmount?: string;      // Intensidad del desenfoque
  opacity?: number;         // Opacidad del fondo
  borderColor?: string;     // Color del borde cristalino
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  blurAmount: '12px',
  opacity: 0.15,
  borderColor: 'rgba(255, 255, 255, 0.3)'
});

const cardStyles = computed(() => ({
  '--glass-blur': props.blurAmount,
  '--glass-opacity': props.opacity,
  '--glass-border': props.borderColor,
  '--glass-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--glass-text-align': props.align,
}));
</script>

<style module>
.glassCard {
  /* Fondo translúcido */
  background: rgba(255, 255, 255, var(--glass-opacity));
  color: #fff; /* Glass suele verse mejor en fondos oscuros/coloridos con texto blanco */
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  
  border-radius: 28px;
  border: 1px solid var(--glass-border);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100%;
  position: relative;
  
  /* Sombra suave para dar profundidad */
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
  transition: all 0.4s ease;
  text-align: var(--glass-text-align);
}

.glassCard:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, calc(var(--glass-opacity) + 0.05));
  border-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 12px 40px 0 rgba(0, 0, 0, 0.3);
}

/* Efecto de brillo de cristal */
.glare {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, rgba(255,255,255,0.15) 0%, rgba(255,255,255,0) 50%);
  pointer-events: none;
}

/* --- ORIENTACIONES --- */
.vertical { flex-direction: column; }
.horizontal { flex-direction: row; }

@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
}

/* --- MEDIA --- */
.imageWrapper {
  position: relative;
  flex-shrink: 0;
  overflow: hidden;
  /* La imagen también se ve "glassy" */
  border-bottom: 1px solid var(--glass-border);
}

.vertical .imageWrapper { height: 200px; }
.horizontal .imageWrapper { width: 40%; border-bottom: none; border-right: 1px solid var(--glass-border); }

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s ease;
}

.glassCard:hover .image {
  transform: scale(1.1);
}

.imageBadge {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: #fff;
  padding: 4px 12px;
  border-radius: 50px;
  font-size: 0.7rem;
  font-weight: 700;
}

/* --- CONTENIDO --- */
.content {
  padding: 30px;
  display: flex;
  flex-direction: column;
  align-items: var(--glass-align);
  justify-content: center;
  gap: 12px;
  flex: 1;
  z-index: 1;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: rgba(255, 255, 255, 0.6);
}

.title {
  margin: 0;
  font-size: 1.6rem;
  font-weight: 700;
  color: #fff;
  text-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.description {
  margin: 0;
  font-size: 0.95rem;
  line-height: 1.5;
  color: rgba(255, 255, 255, 0.8);
}

.footer {
  margin-top: auto;
  padding-top: 20px;
}
</style>