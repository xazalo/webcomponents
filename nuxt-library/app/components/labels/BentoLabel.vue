<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.bentoLabel, 
      $style[variant], 
      $style[size],
      { [$style.isCapsule]: capsule, [$style.hasDot]: dot }
    ]" 
    :style="bentoStyles"
  >
    <span v-if="dot" :class="$style.statusDot" />
    
    <span v-if="$slots.icon" :class="$style.iconArea">
      <slot name="icon" />
    </span>

    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;
  variant?: 'soft' | 'solid' | 'outline' | 'glass';
  size?: 'xs' | 'sm' | 'md';
  capsule?: boolean;      /* Radio totalmente circular */
  dot?: boolean;          /* Añade un punto de status */
  elevation?: number;     /* Intensidad de la sombra (0 a 3) */
  borderRadius?: string;  /* Sobrescribir radio bento */
}

const props = withDefaults(defineProps<Props>(), {
  color: '#4f46e5',
  variant: 'soft',
  size: 'sm',
  capsule: false,
  dot: false,
  elevation: 0
});

const bentoStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-radius': props.capsule ? '9999px' : (props.borderRadius || '0.5rem'),
  '--b-shadow': props.elevation > 0 
    ? `0 ${props.elevation * 2}px ${props.elevation * 4}px rgba(0,0,0,0.08)` 
    : 'none',
}));
</script>

<style module>
.bentoLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  font-family: inherit;
  font-weight: 700;
  line-height: 1;
  border-radius: var(--b-radius);
  box-shadow: var(--b-shadow);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: default;
  white-space: nowrap;
}

/* --- Variantes de Estilo --- */

.soft {
  background: color-mix(in srgb, var(--b-accent) 12%, white);
  color: var(--b-accent);
  border: 1px solid color-mix(in srgb, var(--b-accent) 15%, transparent);
}

.solid {
  background: var(--b-accent);
  color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.outline {
  background: transparent;
  color: var(--b-accent);
  border: 1.5px solid var(--b-accent);
}

.glass {
  background: color-mix(in srgb, var(--b-accent) 10%, transparent);
  backdrop-filter: blur(4px);
  color: var(--b-accent);
  border: 1px solid color-mix(in srgb, var(--b-accent) 20%, transparent);
}

/* --- Tamaños --- */

.xs { padding: 0.2rem 0.5rem; font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.05em; }
.sm { padding: 0.3rem 0.7rem; font-size: 0.75rem; }
.md { padding: 0.45rem 1rem; font-size: 0.85rem; }

/* --- Elementos Extras --- */

.statusDot {
  width: 0.5em;
  height: 0.5em;
  border-radius: 50%;
  background: currentColor;
  flex-shrink: 0;
}

.iconArea {
  display: flex;
  align-items: center;
  font-size: 1.1em;
}

.bentoLabel:hover {
  transform: translateY(-1px);
  filter: contrast(1.1) brightness(1.05);
}

/* Estilo para temas oscuros (opcional, detecta el contexto) */
@media (prefers-color-scheme: dark) {
  .soft {
    background: color-mix(in srgb, var(--b-accent) 20%, #1a1a1a);
  }
}
</style>