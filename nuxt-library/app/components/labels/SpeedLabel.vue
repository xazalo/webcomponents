<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.speedLabel, 
      $style[size], 
      { [$style.isInteractive]: interactive, [$style.animating]: active }
    ]" 
    :style="speedStyles"
  >
    <div v-if="motionBlur" :class="$style.trails">
      <div v-for="i in 3" :key="i" :class="$style.trailLine" />
    </div>

    <span :class="$style.content">
      <slot name="icon" />
      <slot />
    </span>

    <div :class="$style.nitroFlash" />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;         /* Color de la estela y el borde */
  size?: 'xs' | 'sm' | 'md' | 'lg';
  // Configuración Speed
  velocity?: number;      /* Ángulo de inclinación (0 a 30) */
  interactive?: boolean;   /* Habilita desplazamiento y vibración en hover */
  motionBlur?: boolean;   /* Muestra las líneas de estela a la izquierda */
  active?: boolean;       /* Si está activo, la vibración es constante */
  variant?: 'solid' | 'ghost' | 'neon';
}

const props = withDefaults(defineProps<Props>(), {
  color: '#00ff41',
  size: 'sm',
  velocity: 20,
  interactive: true,
  motionBlur: true,
  active: false,
  variant: 'solid'
});

const speedStyles = computed(() => ({
  '--s-accent': props.color,
  '--s-skew': `-${props.velocity}deg`,
  '--s-skew-inv': `${props.velocity / 4}deg`, // Compensación visual suave
  '--s-glow': `${props.color}66`,
}));
</script>

<style module>
.speedLabel {
  display: inline-flex;
  align-items: center;
  position: relative;
  font-family: 'Exo 2', 'Arial Black', sans-serif;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  color: #fff;
  background: #000;
  white-space: nowrap;
  transform: skewX(var(--s-skew));
  border-right: 3px solid var(--s-accent);
  transition: all 0.3s cubic-bezier(0.19, 1, 0.22, 1);
  margin-left: 15px; /* Espacio para las estelas */
}

/* --- Tamaños --- */
.xs { padding: 0.15rem 0.6rem; font-size: 0.6rem; }
.sm { padding: 0.3rem 1rem; font-size: 0.75rem; }
.md { padding: 0.5rem 1.4rem; font-size: 0.95rem; }
.lg { padding: 0.7rem 2rem; font-size: 1.2rem; border-right-width: 5px; }

.content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transform: skewX(var(--s-skew-inv));
  z-index: 2;
  text-shadow: 2px 2px 0px rgba(0, 0, 0, 0.8);
}

/* --- Estelas (Motion Blur) --- */
.trails {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 20px;
  pointer-events: none;
}

.trailLine {
  position: absolute;
  right: 100%;
  height: 2px;
  background: var(--s-accent);
  opacity: 0;
  transition: all 0.3s ease;
}

.trailLine:nth-child(1) { top: 20%; width: 15px; transition-delay: 0.05s; }
.trailLine:nth-child(2) { top: 50%; width: 25px; transition-delay: 0s; }
.trailLine:nth-child(3) { top: 80%; width: 10px; transition-delay: 0.1s; }

/* --- Interacción y Animación --- */
.isInteractive:hover, .animating {
  background: #0a0a0a;
  transform: skewX(var(--s-skew)) translateX(10px);
  box-shadow: -8px 0 20px var(--s-glow);
}

.isInteractive:hover .trailLine, .animating .trailLine {
  opacity: 0.7;
  right: calc(100% + 5px);
}

.isInteractive:hover .content, .animating .content {
  animation: vibration 0.12s infinite linear;
}

.nitroFlash {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent, var(--s-accent), transparent);
  opacity: 0;
  z-index: 1;
  pointer-events: none;
}

.isInteractive:hover .nitroFlash {
  animation: flash 0.6s ease-out;
}

@keyframes vibration {
  0% { transform: skewX(var(--s-skew-inv)) translate(0,0); }
  33% { transform: skewX(var(--s-skew-inv)) translate(1px, -1px); }
  66% { transform: skewX(var(--s-skew-inv)) translate(-1px, 1px); }
  100% { transform: skewX(var(--s-skew-inv)) translate(0,0); }
}

@keyframes flash {
  0% { transform: translateX(-100%); opacity: 0.5; }
  100% { transform: translateX(200%); opacity: 0; }
}
</style>