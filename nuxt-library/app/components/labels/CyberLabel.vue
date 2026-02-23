<template>
  <span 
    v-bind="$attrs" 
    :class="[
      $style.cyberLabel, 
      $style[size], 
      { [$style.glitchAnim]: glitch }
    ]" 
    :style="cyberStyles"
  >
    <div v-if="scanning" :class="$style.scanner" />
    
    <div :class="$style.content">
      <slot name="icon">
        <Icon v-if="icon" :name="icon" :class="$style.icon" />
      </slot>
      <slot />
    </div>

    <div :class="$style.cornerDecoration" />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  color?: string;         /* Color del neón principal */
  icon?: string;
  size?: 'xs' | 'sm' | 'md' | 'lg';
  // Configuración Cyber
  cutSize?: string;       /* Tamaño del chaflán (ej: 8px) */
  glow?: boolean;         /* Habilita el resplandor exterior */
  glitch?: boolean;       /* Animación de parpadeo aleatorio */
  scanning?: boolean;     /* Línea de luz que recorre la etiqueta */
  variant?: 'solid' | 'outline' | 'ghost';
}

const props = withDefaults(defineProps<Props>(), {
  color: '#00f3ff',
  size: 'sm',
  cutSize: '8px',
  glow: true,
  glitch: false,
  scanning: false,
  variant: 'solid'
});

const cyberStyles = computed(() => ({
  '--c-neon': props.color,
  '--c-cut': props.cutSize,
  '--c-glow': props.glow ? `0 0 12px ${props.color}66` : 'none',
  '--c-bg': props.variant === 'solid' ? props.color : (props.variant === 'ghost' ? 'transparent' : 'rgba(0,0,0,0.8)'),
  '--c-text': props.variant === 'solid' ? '#000' : props.color,
  '--c-border': props.variant === 'solid' ? 'none' : `1px solid ${props.color}`
}));
</script>

<style module>
.cyberLabel {
  display: inline-flex;
  align-items: center;
  position: relative;
  font-family: 'Share Tech Mono', 'Courier New', monospace;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--c-text);
  background: var(--c-bg);
  border: var(--c-border);
  box-shadow: var(--c-glow);
  clip-path: polygon(
    var(--c-cut) 0%, 100% 0%, 
    100% calc(100% - var(--c-cut)), 
    calc(100% - var(--c-cut)) 100%, 
    0% 100%, 0% var(--c-cut)
  );
  overflow: hidden;
  white-space: nowrap;
}

/* --- Tamaños --- */
.xs { padding: 0.15rem 0.5rem; font-size: 0.6rem; --c-cut: 4px; }
.sm { padding: 0.25rem 0.8rem; font-size: 0.75rem; }
.md { padding: 0.4rem 1.2rem; font-size: 0.9rem; }
.lg { padding: 0.6rem 1.6rem; font-size: 1.1rem; --c-cut: 12px; }

.content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  z-index: 2;
}

.icon {
  width: 1.1em;
  height: 1.1em;
  filter: drop-shadow(0 0 2px currentColor);
}

/* --- Efecto Scanner --- */
.scanner {
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.4),
    transparent
  );
  transform: skewX(-20deg);
  animation: scan 3s infinite;
  z-index: 1;
}

@keyframes scan {
  0% { left: -100%; }
  30% { left: 150%; }
  100% { left: 150%; }
}

/* --- Animación Glitch --- */
.glitchAnim {
  animation: glitch 4s infinite step-end;
}

@keyframes glitch {
  0%, 90%, 100% { opacity: 1; transform: translateX(0); }
  92% { opacity: 0.8; transform: translateX(2px); filter: hue-rotate(90deg); }
  94% { opacity: 1; transform: translateX(-2px); }
  96% { opacity: 0.5; filter: blur(1px); }
}

/* --- Decoración de Esquina --- */
.cornerDecoration {
  position: absolute;
  top: 0;
  right: 0;
  width: 4px;
  height: 4px;
  background: white;
  opacity: 0.5;
  clip-path: circle(50%);
}
</style>