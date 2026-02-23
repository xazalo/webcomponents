<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.neuLabel, 
      $style[size], 
      $style[appearance],
      { [$style.isInteractive]: interactive }
    ]" 
    :style="neuStyles"
  >
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
  color?: string;         /* Color del texto */
  baseColor?: string;     /* Color de la superficie (debe ser el mismo que el fondo) */
  size?: 'xs' | 'sm' | 'md';
  appearance?: 'raised' | 'pressed'; /* ¿Sobresale o está hundido? */
  intensity?: number;     /* Opacidad de las sombras (0.1 a 1) */
  distance?: number;      /* Qué tanto sobresale en px */
  interactive?: boolean;   /* ¿Cambia al pasar el mouse? */
  radius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  color: '#6d7c90',
  baseColor: '#e0e5ec',
  size: 'sm',
  appearance: 'raised',
  intensity: 0.6,
  distance: 4,
  interactive: true,
  radius: '10px'
});

const neuStyles = computed(() => {
  const d = props.distance;
  const b = d * 2; // El desenfoque suele ser el doble de la distancia
  
  return {
    '--n-bg': props.baseColor,
    '--n-text': props.color,
    '--n-radius': props.radius,
    '--n-shadow-dark': `rgba(163, 177, 198, ${props.intensity})`,
    '--n-shadow-light': `rgba(255, 255, 255, ${props.intensity + 0.2})`,
    '--n-d': `${d}px`,
    '--n-d-neg': `-${d}px`,
    '--n-b': `${b}px`,
  };
});
</script>

<style module>
.neuLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-family: inherit;
  font-weight: 700;
  color: var(--n-text);
  background: var(--n-bg);
  border-radius: var(--n-radius);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: default;
  user-select: none;
  white-space: nowrap;
  border: 1px solid rgba(255, 255, 255, 0.1); /* Brillo de borde casi invisible */
}

/* --- Estados de Relieve --- */

.raised {
  box-shadow: 
    var(--n-d) var(--n-d) var(--n-b) var(--n-shadow-dark),
    var(--n-d-neg) var(--n-d-neg) var(--n-b) var(--n-shadow-light);
}

.pressed {
  box-shadow: 
    inset var(--n-d) var(--n-d) var(--n-b) var(--n-shadow-dark),
    inset var(--n-d-neg) var(--n-d-neg) var(--n-b) var(--n-shadow-light);
}

/* --- Tamaños --- */
.xs { padding: 0.2rem 0.6rem; font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.05em; }
.sm { padding: 0.4rem 1rem; font-size: 0.75rem; }
.md { padding: 0.6rem 1.4rem; font-size: 0.9rem; }

/* --- Interacción --- */
.isInteractive.raised:hover {
  transform: translateY(-2px);
  box-shadow: 
    calc(var(--n-d) + 2px) calc(var(--n-d) + 2px) calc(var(--n-b) + 4px) var(--n-shadow-dark),
    calc(var(--n-d-neg) - 2px) calc(var(--n-d-neg) - 2px) calc(var(--n-b) + 4px) var(--n-shadow-light);
}

.isInteractive.pressed:hover {
  filter: brightness(0.98);
}

.iconArea {
  display: flex;
  align-items: center;
  font-size: 1.1em;
  opacity: 0.8;
}
</style>