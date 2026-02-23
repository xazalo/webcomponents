<template>
  <div 
    :class="[
      $style.neuCard, 
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
  // Props Neumorphism
  baseColor?: string;    // Debe ser el mismo que el fondo del contenedor
  accentColor?: string;  // Para detalles como el Tag
  distance?: number;     // Qué tanto se "eleva" la tarjeta
  blur?: number;         // Suavidad de la elevación
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  baseColor: '#e0e5ec', // Gris azulado clásico neu
  accentColor: '#6d5dfc',
  distance: 10,
  blur: 20
});

const cardStyles = computed(() => {
  const d = props.distance;
  const b = props.blur;
  
  return {
    '--neu-bg': props.baseColor,
    '--neu-accent': props.accentColor,
    '--neu-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
    '--neu-text-align': props.align,
    // Sombras: una positiva oscura y una negativa clara
    '--neu-shadow-flat': `${d}px ${d}px ${b}px #a3b1c6, -${d}px -${d}px ${b}px #ffffff`,
    '--neu-shadow-hover': `${d+2}px ${d+2}px ${b+5}px #a3b1c6, -${d+2}px -${d+2}px ${b+5}px #ffffff`,
    '--neu-shadow-inset': `inset 2px 2px 5px #a3b1c6, inset -5px -5px 10px #ffffff`
  };
});
</script>

<style module>
.neuCard {
  background: var(--neu-bg);
  border-radius: 40px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  height: 100%;
  box-shadow: var(--neu-shadow-flat);
  transition: all 0.3s ease;
  text-align: var(--neu-text-align);
  padding: 10px; /* Margen para que se vea la extrusión */
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.neuCard:hover {
  transform: translateY(-2px);
  box-shadow: var(--neu-shadow-hover);
}

/* --- ORIENTACIONES --- */
.vertical { flex-direction: column; }
.horizontal { flex-direction: row; }

@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
}

/* --- MEDIA (HUNDIDA) --- */
.imageWrapper {
  position: relative;
  overflow: hidden;
  margin: 10px;
  border-radius: 30px;
  flex-shrink: 0;
  /* La imagen parece estar "dentro" de la tarjeta */
  box-shadow: var(--neu-shadow-inset);
}

.vertical .imageWrapper { height: 180px; }
.horizontal .imageWrapper { width: 40%; margin-right: 0; }

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  mix-blend-mode: multiply; /* Para fundirse con el fondo si es posible */
  opacity: 0.9;
}

.imageBadge {
  position: absolute;
  top: 15px;
  right: 15px;
  background: var(--neu-bg);
  box-shadow: 3px 3px 6px #a3b1c6, -3px -3px 6px #ffffff;
  padding: 5px 12px;
  border-radius: 50px;
  font-size: 0.65rem;
  font-weight: 800;
  color: var(--neu-accent);
}

/* --- CONTENIDO --- */
.content {
  padding: 25px;
  display: flex;
  flex-direction: column;
  align-items: var(--neu-align);
  justify-content: center;
  gap: 12px;
  flex: 1;
}

.tag {
  color: var(--neu-accent);
  font-size: 0.75rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.title {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 800;
  color: #4a5568;
}

.description {
  margin: 0;
  font-size: 0.9rem;
  line-height: 1.5;
  color: #718096;
  font-weight: 600;
}

.footer {
  margin-top: auto;
  padding-top: 15px;
}

@media (max-width: 768px) {
  .horizontal .imageWrapper { width: auto; margin-right: 10px; }
}
</style>