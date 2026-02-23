<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.cyberRoot, $style[size], { [$style.disabled]: disabled }]" 
    :style="cyberStyles"
  >
    <label v-if="label" :class="$style.label">
      <span :class="$style.labelPrefix">::</span> {{ label }}
    </label>
    
    <div :class="$style.container">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :type="type"
        :disabled="disabled"
        :class="[$style.field, { [$style.hasError]: error }]"
      />
      
      <div :class="$style.cornerDetail"></div>
      
      <div :class="$style.scannerLine"></div>
    </div>
    
    <div :class="$style.footer">
      <span v-if="error" :class="$style.errorText">! ERROR_DETECTED: {{ error }}</span>
      <span v-else-if="modelValue" :class="$style.statusText">INPUT_STABLE</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: string | number;
  label?: string;
  placeholder?: string;
  type?: string;
  error?: string;
  disabled?: boolean;
  // Personalización Cyber
  neonColor?: string;     /* Color del neón (ej: #00f3ff) */
  bgColor?: string;       /* Fondo del input */
  cutSize?: number;       /* Tamaño del corte en px */
  glowIntensity?: number; /* De 0 a 1 */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Esperando datos...',
  neonColor: '#00f3ff',
  bgColor: '#0f0f0f',
  cutSize: 10,
  glowIntensity: 0.5,
  size: 'md'
});

defineEmits(['update:modelValue']);

const cyberStyles = computed(() => ({
  '--c-neon': props.neonColor,
  '--c-bg': props.bgColor,
  '--c-cut': `${props.cutSize}px`,
  '--c-glow': `${props.neonColor}${Math.floor(props.glowIntensity * 255).toString(16).padStart(2, '0')}`,
  '--c-shadow': `0 0 ${20 * props.glowIntensity}px ${props.neonColor}44`,
}));
</script>

<style module>
.cyberRoot {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  width: 100%;
}

.label {
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.75rem;
  font-weight: 900;
  text-transform: uppercase;
  color: #fff;
  letter-spacing: 0.15em;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.labelPrefix {
  color: var(--c-neon);
  animation: blink 1s infinite;
}

.container {
  position: relative;
  background: #1a1a1a;
  /* El clip-path usa la variable --c-cut para ser dinámico */
  clip-path: polygon(
    var(--c-cut) 0%, 100% 0%, 
    100% calc(100% - var(--c-cut)), 
    calc(100% - var(--c-cut)) 100%, 
    0% 100%, 0% var(--c-cut)
  );
  transition: transform 0.2s ease;
}

.field {
  width: 100%;
  background: var(--c-bg);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #fff;
  font-family: 'Courier New', Courier, monospace;
  font-weight: 600;
  outline: none;
  transition: all 0.3s ease;
  clip-path: inherit;
}

/* Tamaños */
.sm .field { padding: 0.6rem 1rem; font-size: 0.8rem; }
.md .field { padding: 0.9rem 1.4rem; font-size: 1rem; }
.lg .field { padding: 1.2rem 1.8rem; font-size: 1.3rem; }

.field:focus {
  border-color: var(--c-neon);
  box-shadow: inset 0 0 10px var(--c-glow);
  background: #000;
}

.field::placeholder {
  color: rgba(255, 255, 255, 0.2);
  text-transform: uppercase;
  font-size: 0.8em;
}

/* Detalles decorativos */
.cornerDetail {
  position: absolute;
  top: 0;
  right: 0;
  width: 20px;
  height: 2px;
  background: var(--c-neon);
  opacity: 0;
  transition: opacity 0.3s;
}

.field:focus + .cornerDetail {
  opacity: 1;
  box-shadow: 0 0 10px var(--c-neon);
}

.scannerLine {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0%;
  height: 2px;
  background: var(--c-neon);
  box-shadow: 0 0 15px var(--c-neon);
  transition: width 0.4s cubic-bezier(0.23, 1, 0.32, 1);
}

.field:focus ~ .scannerLine {
  width: 100%;
}

.footer {
  display: flex;
  justify-content: space-between;
  min-height: 1rem;
}

.errorText {
  font-family: monospace;
  font-size: 0.65rem;
  color: #ff0055;
  font-weight: 900;
}

.statusText {
  font-family: monospace;
  font-size: 0.65rem;
  color: var(--c-neon);
  opacity: 0.6;
}

.hasError {
  border-color: #ff0055 !important;
  box-shadow: inset 0 0 10px rgba(255, 0, 85, 0.2) !important;
}

@keyframes blink {
  50% { opacity: 0; }
}

.disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>