<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.cyberRoot, { [$style.isActive]: modelValue }]"
    :style="cyberStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div v-if="scanlines" :class="$style.scanline"></div>
      <div :class="[$style.cyberIndicator, modelValue ? $style.bgCyber : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.controlCell]">
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
      <span :class="$style.statusText">
        {{ modelValue ? onText : offText }}
      </span>
    </div>

    <div :class="$style.edgeDecor"></div>
  </button>
</template>

<script setup lang="ts">
import { computed } from 'vue';

defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label: string;
  onText?: string;
  offText?: string;
  // Cyber Tuning
  primaryColor?: string;   /* Color principal (ej: Amarillo Cyber) */
  accentColor?: string;    /* Color de acción (ej: Cyan) */
  glitchColor?: string;    /* Color de contraste (ej: Neon Pink) */
  scanlines?: boolean;     /* ¿Efecto de monitor antiguo? */
  angle?: number;          /* Ángulo de los cortes (px) */
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'LINK_ESTABLISHED',
  offText: 'DISCONNECTED',
  primaryColor: '#fcee0a',
  accentColor: '#00f0ff',
  glitchColor: '#ff003c',
  scanlines: true,
  angle: 10
});

defineEmits(['update:modelValue']);

const cyberStyles = computed(() => ({
  '--cb-primary': props.primaryColor,
  '--cb-accent': props.accentColor,
  '--cb-glitch': props.glitchColor,
  '--cb-angle': `${props.angle}px`,
  '--cb-angle-neg': `-${props.angle}px`,
}));
</script>

<style module>
.cyberRoot {
  display: flex;
  gap: 10px;
  padding: 8px;
  background: #050505;
  border: 2px solid var(--cb-primary);
  /* Corte angular dinámico */
  clip-path: polygon(
    0 0, 
    calc(100% - var(--cb-angle)) 0, 
    100% var(--cb-angle), 
    100% 100%, 
    var(--cb-angle) 100%, 
    0 calc(100% - var(--cb-angle))
  );
  cursor: pointer;
  position: relative;
  transition: all 0.2s steps(4); /* Transición segmentada tipo digital */
  width: fit-content;
  outline: none;
}

.cyberRoot:hover {
  filter: brightness(1.2);
  box-shadow: 0 0 15px color-mix(in srgb, var(--cb-primary), transparent 60%);
}

.cyberRoot:active {
  transform: translateY(2px) scale(0.98);
}

.cell {
  background: #111;
  padding: 10px 18px;
  border: 1px solid #222;
  display: flex;
  align-items: center;
  gap: 12px;
  position: relative;
  overflow: hidden;
  clip-path: polygon(0 0, 100% 0, 100% 100%, var(--cb-angle) 100%, 0 calc(100% - var(--cb-angle)));
}

.scanline {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(255, 255, 255, 0.03) 50%);
  background-size: 100% 4px;
  pointer-events: none;
  animation: scroll 10s linear infinite;
}

@keyframes scroll {
  from { background-position: 0 0; }
  to { background-position: 0 100%; }
}

.label {
  font-family: 'Consolas', 'Courier New', monospace;
  font-size: 0.75rem;
  font-weight: 900;
  color: var(--cb-primary);
  text-transform: uppercase;
  letter-spacing: 2px;
  /* Efecto Aberración Cromática */
  text-shadow: 2px 1px 0px var(--cb-glitch);
}

.cyberIndicator {
  width: 15px;
  height: 4px;
  background: #333;
  transition: all 0.3s ease;
}

.bgCyber {
  background: var(--cb-accent);
  box-shadow: 0 0 12px var(--cb-accent);
}

.bgOff { background: #333; }

.controlCell {
  min-width: 160px;
  justify-content: space-between;
}

.track {
  width: 44px;
  height: 14px;
  background: #000;
  border: 1px solid #333;
  position: relative;
}

.thumb {
  width: 18px;
  height: 100%;
  background: #333;
  position: absolute;
  top: 0; left: 0;
  transition: all 0.2s cubic-bezier(0.8, 0, 0.2, 1);
}

.statusText {
  font-family: 'Consolas', monospace;
  font-size: 0.65rem;
  font-weight: 700;
  color: #555;
  text-transform: uppercase;
}

.edgeDecor {
  position: absolute;
  right: 4px; top: 20%;
  width: 2px; height: 30%;
  background: var(--cb-glitch);
}

/* --- ESTADO ACTIVO --- */

.isActive {
  border-color: var(--cb-accent);
}

.isActive .label {
  color: var(--cb-accent);
  text-shadow: -2px -1px 0px var(--cb-glitch);
}

.isActive .track {
  border-color: var(--cb-accent);
}

.isActive .thumb {
  transform: translateX(24px);
  background: var(--cb-glitch);
  box-shadow: -5px 0 15px var(--cb-glitch);
}

.isActive .statusText {
  color: var(--cb-accent);
  animation: blink 0.4s infinite alternate;
}

@keyframes blink {
  from { opacity: 1; text-shadow: 0 0 5px var(--cb-accent); }
  to { opacity: 0.4; text-shadow: 0 0 0px var(--cb-accent); }
}
</style>