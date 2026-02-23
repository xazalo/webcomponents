<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.speedWrapper, 
      $style[size], 
      { [$style.checked]: modelValue, [$style.disabled]: disabled }
    ]" 
    :style="speedStyles"
  >
    <div :class="$style.motionBlur" />

    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        :disabled="disabled"
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.speedBox">
        <Transition name="zap-pop">
          <svg v-if="modelValue" viewBox="0 0 24 24" :class="$style.checkIcon">
            <path d="M13 2L3 14H12L11 22L21 10H12L13 2Z" fill="currentColor" />
          </svg>
        </Transition>
      </div>
    </div>

    <span v-if="label" :class="$style.label">
      {{ label }}
    </span>

    <div :class="$style.nitroLine" />
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label?: string;
  disabled?: boolean;
  // Estética Speed
  accentColor?: string;   /* Color de la energía/nitro */
  baseColor?: string;     /* Color del chasis (fondo) */
  skewAngle?: number;     /* Grados de inclinación */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  disabled: false,
  accentColor: '#ff004c',
  baseColor: '#0f0f0f',
  skewAngle: 15,
  size: 'md'
});

defineEmits(['update:modelValue']);

const speedStyles = computed(() => ({
  '--s-accent': props.accentColor,
  '--s-bg': props.baseColor,
  '--s-skew': `-${props.skewAngle}deg`,
  '--s-skew-inv': `${props.skewAngle}deg`,
  '--s-glow': `${props.accentColor}66`,
}));
</script>

<style module>
.speedWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.5rem 1.2rem;
  background: var(--s-bg);
  border-radius: 2px;
  position: relative;
  overflow: hidden;
  user-select: none;
  transform: skewX(var(--s-skew));
  border-left: 3px solid transparent;
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

.inputContainer {
  display: flex;
  transform: skewX(var(--s-skew-inv));
}

.inputContainer input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.speedBox {
  width: 1.2em;
  height: 1.2em;
  background: #2a2a2a;
  border: 1px solid #444;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  transform: rotate(-5deg); /* Un toque de asimetría */
}

/* Estado Marcado */
.checked {
  background: #1a1a1a;
  border-left-color: var(--s-accent);
  box-shadow: -5px 0 15px var(--s-glow);
}

.checked .speedBox {
  background: var(--s-accent);
  border-color: #fff;
  transform: rotate(0deg) scale(1.1);
  box-shadow: 0 0 15px var(--s-accent);
}

.checkIcon {
  color: white;
  width: 80%;
  height: 80%;
}

.label {
  font-size: 0.9em;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  color: #888;
  letter-spacing: 0.05em;
  transform: skewX(var(--s-skew-inv));
  transition: all 0.3s ease;
}

.checked .label {
  color: #fff;
  text-shadow: 0 0 8px var(--s-accent);
}

/* Nitro Line (Hover Effect) */
.nitroLine {
  position: absolute;
  bottom: 0;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--s-accent), transparent);
  transition: left 0.5s ease-in;
}

/* Motion Blur Effect */
.motionBlur {
  position: absolute;
  inset: 0;
  background: var(--s-accent);
  opacity: 0;
  transition: opacity 0.3s ease;
  mix-blend-mode: overlay;
}

/* Hover & Active States */
.speedWrapper:hover:not(.disabled) {
  transform: skewX(var(--s-skew)) translateX(8px);
  background: #151515;
}

.speedWrapper:hover .nitroLine {
  left: 100%;
}

.speedWrapper:active:not(.disabled) {
  transform: skewX(var(--s-skew)) scale(0.95) translateX(4px);
}

/* Tamaños */
.sm { font-size: 12px; }
.md { font-size: 16px; }
.lg { font-size: 22px; }

.disabled {
  opacity: 0.3;
  filter: grayscale(1);
  cursor: not-allowed;
}

/* Animación del Rayo (Zap) */
:global(.zap-pop-enter-active) {
  animation: zap-in 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}
@keyframes zap-in {
  0% { transform: scale(0) translateY(10px); opacity: 0; }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}
</style>