<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.clayLabel, 
      $style[size], 
      { [$style.isInteractive]: interactive }
    ]" 
    :style="clayStyles"
  >
    <slot />
    
    <div v-if="gloss" :class="$style.gloss" />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;         /* Color base de la arcilla */
  textColor?: string;     /* Color del texto */
  size?: 'xs' | 'sm' | 'md' | 'lg';
  depth?: number;         /* Intensidad del relieve (0.5 a 2) */
  radius?: string;        /* Curvatura personalizada */
  interactive?: boolean;   /* Habilita escala en hover */
  gloss?: boolean;         /* El puntito de brillo blanco */
}

const props = withDefaults(defineProps<Props>(), {
  color: '#a855f7',
  textColor: '#ffffff',
  size: 'sm',
  depth: 1,
  radius: '12px',
  interactive: true,
  gloss: true
});

const clayStyles = computed(() => ({
  '--clay-bg': props.color,
  '--clay-text': props.textColor,
  '--clay-radius': props.radius,
  // Cálculo de sombras dinámicas basadas en la profundidad
  '--clay-shadow-ext': `0 ${4 * props.depth}px ${10 * props.depth}px rgba(0, 0, 0, 0.1)`,
  '--clay-shadow-int-top': `inset 0 ${3 * props.depth}px ${5 * props.depth}px rgba(255, 255, 255, 0.45)`,
  '--clay-shadow-int-bot': `inset 0 -${3 * props.depth}px ${5 * props.depth}px rgba(0, 0, 0, 0.25)`,
}));
</script>

<style module>
.clayLabel {
  display: inline-flex;
  align-items: center;
  position: relative;
  font-family: inherit;
  font-weight: 800;
  color: var(--clay-text);
  background: var(--clay-bg);
  border-radius: var(--clay-radius);
  white-space: nowrap;
  user-select: none;
  cursor: default;
  
  /* El efecto Clay base */
  box-shadow: 
    var(--clay-shadow-ext),
    var(--clay-shadow-int-top),
    var(--clay-shadow-int-bot);
    
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* --- Tamaños --- */
.xs { padding: 0.15rem 0.5rem; font-size: 0.65rem; letter-spacing: 0.02em; }
.sm { padding: 0.3rem 0.9rem; font-size: 0.75rem; }
.md { padding: 0.5rem 1.2rem; font-size: 0.9rem; }
.lg { padding: 0.7rem 1.6rem; font-size: 1.1rem; }

/* --- Interacción --- */
.isInteractive:hover {
  transform: scale(1.08) translateY(-2px);
  filter: brightness(1.05);
  box-shadow: 
    0 10px 20px rgba(0, 0, 0, 0.12),
    var(--clay-shadow-int-top),
    var(--clay-shadow-int-bot);
}

.isInteractive:active {
  transform: scale(0.95);
}

/* --- Reflejo (Highlight) --- */
.gloss {
  position: absolute;
  top: 15%;
  left: 12%;
  width: 15%;
  height: 20%;
  background: rgba(255, 255, 255, 0.35);
  border-radius: 50%;
  filter: blur(1px);
  pointer-events: none;
}

/* Ajuste para iconos dentro del slot */
.clayLabel :global(svg) {
  width: 1.2em;
  height: 1.2em;
  margin-right: 0.4em;
  filter: drop-shadow(0 1px 1px rgba(0,0,0,0.1));
}
</style>