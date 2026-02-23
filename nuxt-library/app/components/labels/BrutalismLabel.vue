<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.brutalLabel, 
      $style[size], 
      $style[variant],
      { [$style.isInteractive]: interactive }
    ]" 
    :style="brutalStyles"
  >
    <span v-if="$slots.icon" :class="$style.iconArea">
      <slot name="icon" />
    </span>

    <slot />

    <div v-if="glitch" :class="$style.glitchLine" />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;
  shadowColor?: string;
  variant?: 'solid' | 'outline' | 'reverse';
  size?: 'xs' | 'sm' | 'md' | 'lg';
  // Personalización Brutal
  borderWidth?: string;
  shadowSize?: string;
  rotation?: number;       /* Grados de inclinación (ej: -3) */
  interactive?: boolean;   /* Habilita efectos de hover/click */
  glitch?: boolean;        /* Añade un detalle visual extra */
}

const props = withDefaults(defineProps<Props>(), {
  color: '#ffff00',
  shadowColor: '#000000',
  variant: 'solid',
  size: 'sm',
  borderWidth: '2px',
  shadowSize: '4px',
  rotation: 0,
  interactive: true,
  glitch: false
});

const brutalStyles = computed(() => ({
  '--br-accent': props.color,
  '--br-shadow-col': props.shadowColor,
  '--br-bw': props.borderWidth,
  '--br-ss': props.shadowSize,
  '--br-rot': `${props.rotation}deg`,
}));
</script>

<style module>
.brutalLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-family: 'Inter', 'Arial Black', sans-serif;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.02em;
  border-radius: 2px;
  position: relative;
  transform: rotate(var(--br-rot));
  transition: all 0.15s cubic-bezier(0.18, 0.89, 0.32, 1.28);
  white-space: nowrap;
  user-select: none;
}

/* --- Variantes --- */

.solid {
  background: var(--br-accent);
  color: #000;
  border: var(--br-bw) solid #000;
  box-shadow: var(--br-ss) var(--br-ss) 0px 0px var(--br-shadow-col);
}

.outline {
  background: #fff;
  color: #000;
  border: var(--br-bw) solid #000;
  box-shadow: var(--br-ss) var(--br-ss) 0px 0px var(--br-accent);
}

.reverse {
  background: #000;
  color: #fff;
  border: var(--br-bw) solid var(--br-accent);
  box-shadow: var(--br-ss) var(--br-ss) 0px 0px var(--br-accent);
}

/* --- Tamaños --- */

.xs { padding: 0.1rem 0.4rem; font-size: 0.65rem; --br-ss: 2px; }
.sm { padding: 0.25rem 0.75rem; font-size: 0.85rem; }
.md { padding: 0.4rem 1.2rem; font-size: 1.1rem; }
.lg { padding: 0.6rem 1.5rem; font-size: 1.4rem; --br-bw: 4px; --br-ss: 8px; }

/* --- Interacción --- */

.isInteractive {
  cursor: pointer;
}

.isInteractive:hover {
  transform: rotate(var(--br-rot)) translate(-2px, -2px);
  box-shadow: calc(var(--br-ss) + 2px) calc(var(--br-ss) + 2px) 0px 0px var(--br-shadow-col);
}

.isInteractive:active {
  transform: rotate(var(--br-rot)) translate(var(--br-ss), var(--br-ss));
  box-shadow: 0px 0px 0px 0px var(--br-shadow-col);
}

.iconArea {
  display: flex;
  align-items: center;
  font-size: 1.2em;
}

.glitchLine {
  position: absolute;
  right: 4px;
  top: 20%;
  bottom: 20%;
  width: 4px;
  background: #000;
  opacity: 0.3;
}
</style>