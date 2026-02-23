<template>
  <span 
    v-bind="$attrs" 
    :class="[$style.glassLabel, $style[size]]" 
    :style="glassStyles"
  >
    <div v-if="dot" :class="$style.dotContainer">
      <div :class="$style.dot" />
      <div :class="$style.dotPulse" />
    </div>

    <span v-if="$slots.icon" :class="$style.iconArea">
      <slot name="icon" />
    </span>

    <span :class="$style.text">
      <slot />
    </span>
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  dot?: boolean;
  dotColor?: string;
  size?: 'xs' | 'sm' | 'md';
  // Configuración Glass
  tintColor?: string;     /* Color del tinte (RGB ej: "255, 255, 255") */
  opacity?: number;       /* Opacidad base (0 a 1) */
  blur?: string;          /* Intensidad del desenfoque */
  shadow?: string | boolean;
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  dot: false,
  dotColor: '#f59e0b',
  size: 'sm',
  tintColor: '255, 255, 255',
  opacity: 0.45,
  blur: '8px',
  borderRadius: '8px'
});

const glassStyles = computed(() => ({
  '--g-bg': `rgba(${props.tintColor}, ${props.opacity})`,
  '--g-blur': props.blur,
  '--g-radius': props.borderRadius,
  '--g-dot': props.dotColor,
  '--g-shadow': props.shadow === true 
    ? "0 8px 32px 0 rgba(31, 38, 135, 0.07)" 
    : (props.shadow || 'none'),
  '--g-border': `rgba(${props.tintColor}, 0.5)`,
  '--g-edge': `rgba(${props.tintColor}, 0.8)`,
}));
</script>

<style module>
.glassLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  font-family: inherit;
  font-weight: 600;
  color: #1f2937;
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur));
  -webkit-backdrop-filter: blur(var(--g-blur));
  border-radius: var(--g-radius);
  box-shadow: var(--g-shadow);
  position: relative;
  white-space: nowrap;
  
  /* Borde de cristal: Gradiente sutil que simula luz desde arriba */
  border: 1px solid var(--g-border);
  border-top-color: var(--g-edge);
}

/* --- Tamaños --- */
.xs { padding: 0.15rem 0.6rem; font-size: 0.65rem; }
.sm { padding: 0.3rem 0.8rem; font-size: 0.75rem; }
.md { padding: 0.45rem 1rem; font-size: 0.85rem; }

/* --- Punto de Estado y Pulso --- */
.dotContainer {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 8px;
  height: 8px;
}

.dot {
  width: 6px;
  height: 6px;
  background: var(--g-dot);
  border-radius: 50%;
  z-index: 2;
}

.dotPulse {
  position: absolute;
  width: 100%;
  height: 100%;
  background: var(--g-dot);
  border-radius: 50%;
  opacity: 0.6;
  animation: pulse 2s infinite ease-out;
  z-index: 1;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 0.6; }
  100% { transform: scale(3); opacity: 0; }
}

.iconArea {
  display: flex;
  align-items: center;
  opacity: 0.8;
  font-size: 1.1em;
}

.text {
  position: relative;
  z-index: 2;
}

/* Efecto Hover: Claridad del cristal */
.glassLabel:hover {
  background: rgba(255, 255, 255, 0.6);
  border-color: rgba(255, 255, 255, 0.8);
  transform: translateY(-1px);
}
</style>