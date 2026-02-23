<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.glassWrapper, 
      $style[size], 
      { [$style.checked]: modelValue, [$style.disabled]: disabled }
    ]" 
    :style="glassStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        :disabled="disabled"
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.glassBox">
        <Transition name="glass-fade">
          <svg v-if="modelValue" viewBox="0 0 24 24" :class="$style.checkIcon">
            <path d="M20 6L9 17L4 12" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </Transition>
      </div>
    </div>
    <span v-if="label" :class="$style.label">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label?: string;
  disabled?: boolean;
  // Estética Glass
  accentColor?: string;   /* Color del check y resplandor */
  blurAmount?: string;    /* Intensidad del desenfoque */
  opacity?: number;       /* Opacidad base del cristal */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  disabled: false,
  accentColor: '#ffffff',
  blurAmount: '12px',
  opacity: 0.15,
  size: 'md'
});

defineEmits(['update:modelValue']);

const glassStyles = computed(() => ({
  '--g-blur': props.blurAmount,
  '--g-bg': `rgba(255, 255, 255, ${props.opacity})`,
  '--g-bg-active': `rgba(255, 255, 255, ${props.opacity + 0.15})`,
  '--g-accent': props.accentColor,
  '--g-glow': `${props.accentColor}4D`, // 30% de opacidad
}));
</script>

<style module>
.glassWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  user-select: none;
  padding: 0.5rem 0.8rem;
  border-radius: 1rem;
  transition: all 0.3s ease;
}

.glassWrapper:hover:not(.disabled) {
  background: rgba(255, 255, 255, 0.08);
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.glassBox {
  width: 1.4em;
  height: 1.4em;
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur));
  -webkit-backdrop-filter: blur(var(--g-blur));
  border-radius: 0.5em;
  border: 1px solid rgba(255, 255, 255, 0.25);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  box-shadow: 
    0 4px 12px rgba(0, 0, 0, 0.1),
    inset 0 0 0 1px rgba(255, 255, 255, 0.1);
}

/* Estado Marcado */
.checked .glassBox {
  background: var(--g-bg-active);
  border-color: rgba(255, 255, 255, 0.5);
  transform: scale(1.1);
  box-shadow: 
    0 8px 20px rgba(0, 0, 0, 0.15),
    0 0 15px var(--g-glow);
}

.checkIcon {
  color: var(--g-accent);
  width: 70%;
  height: 70%;
  filter: drop-shadow(0 0 4px var(--g-glow));
}

.label {
  color: #ffffff;
  font-weight: 600;
  font-size: 0.95rem;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

/* Tamaños */
.sm { font-size: 14px; }
.md { font-size: 18px; }
.lg { font-size: 24px; }

.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Animación de entrada suave */
:global(.glass-fade-enter-active), 
:global(.glass-fade-leave-active) {
  transition: all 0.3s ease;
}
:global(.glass-fade-enter-from), 
:global(.glass-fade-leave-to) {
  opacity: 0;
  transform: scale(0.5);
}

.glassWrapper:active:not(.disabled) .glassBox {
  transform: scale(0.9);
}
</style>