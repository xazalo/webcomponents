<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.bubbleLabel, 
      $style[size], 
      { [$style.isInteractive]: interactive, [$style.hasGloss]: gloss }
    ]" 
    :style="bubbleStyles"
  >
    <div v-if="gloss" :class="$style.reflection" />

    <span v-if="$slots.icon" :class="$style.iconArea">
      <slot name="icon" />
    </span>

    <span :class="$style.content">
      <slot />
    </span>
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;
  size?: 'xs' | 'sm' | 'md' | 'lg';
  interactive?: boolean;   /* Habilita animaciones de hover */
  gloss?: boolean;         /* Añade el reflejo de luz superior */
  jelly?: boolean;         /* Activa la animación elástica al entrar */
  roundness?: number;      /* Control de la asimetría (0 a 2) */
  elevation?: number;      /* Intensidad de la sombra de apoyo */
}

const props = withDefaults(defineProps<Props>(), {
  color: '#10b981',
  size: 'sm',
  interactive: true,
  gloss: true,
  jelly: true,
  roundness: 1,
  elevation: 1
});

const bubbleStyles = computed(() => ({
  '--bb-color': props.color,
  '--bb-radius-top': `${25 * props.roundness}px`,
  '--bb-radius-bot': `${10 * props.roundness}px`,
  '--bb-shadow': `0 ${props.elevation * 4}px ${props.elevation * 10}px -2px rgba(0,0,0,0.15)`,
  '--bb-glow': `inset 0 -4px 8px rgba(0,0,0,0.1), inset 0 4px 8px rgba(255,255,255,0.3)`
}));
</script>

<style module>
.bubbleLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  position: relative;
  font-family: 'Rounded Mplus 1c', 'Inter', sans-serif;
  font-weight: 800;
  color: white;
  background: var(--bb-color);
  border-radius: var(--bb-radius-top) var(--bb-radius-bot) var(--bb-radius-top) var(--bb-radius-bot);
  box-shadow: var(--bb-shadow), var(--bb-glow);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  cursor: default;
  user-select: none;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

/* --- Tamaños --- */
.xs { padding: 0.15rem 0.6rem; font-size: 0.65rem; }
.sm { padding: 0.3rem 0.9rem; font-size: 0.75rem; }
.md { padding: 0.5rem 1.2rem; font-size: 0.9rem; }
.lg { padding: 0.7rem 1.5rem; font-size: 1.1rem; }

/* --- Efectos Visuales --- */
.reflection {
  position: absolute;
  top: 15%;
  left: 10%;
  width: 30%;
  height: 25%;
  background: rgba(255, 255, 255, 0.4);
  border-radius: 50%;
  filter: blur(2px);
  pointer-events: none;
}

.content {
  position: relative;
  z-index: 2;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.iconArea {
  display: flex;
  align-items: center;
  z-index: 2;
}

/* --- Animaciones --- */
.isInteractive:hover {
  transform: scale(1.1) rotate(2deg);
  border-radius: var(--bb-radius-bot) var(--bb-radius-top) var(--bb-radius-bot) var(--bb-radius-top);
  filter: brightness(1.05);
}

.isInteractive:active {
  transform: scale(0.95);
}

/* Animación Jelly (al aparecer) */
@keyframes jelly-in {
  0% { transform: scale(0.5); }
  45% { transform: scale(1.1); }
  70% { transform: scale(0.9); }
  100% { transform: scale(1); }
}

.bubbleLabel {
  animation: jelly-in 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
</style>