<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.bubbleWrapper, 
      $style[size], 
      { [$style.checked]: modelValue }
    ]" 
    :style="bubbleStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.bubbleCheck">
        <Transition name="jelly">
          <div v-if="modelValue" :class="$style.innerDot" />
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
  // Personalización de Color
  accentColor?: string;   /* Color principal de la burbuja activa */
  baseColor?: string;     /* Color de la burbuja reposo */
  textColor?: string;     /* Color de la etiqueta */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  shadowIntensity?: number; /* 0 a 1 */
  elasticity?: number;    /* Qué tanto cambia de forma (0 a 1) */
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  accentColor: '#f43f5e',   /* Un rosa chicle muy Bubble */
  baseColor: '#e5e7eb',
  textColor: '#4b5563',
  size: 'md',
  shadowIntensity: 0.15,
  elasticity: 1
});

defineEmits(['update:modelValue']);

const bubbleStyles = computed(() => ({
  '--bubble-accent': props.accentColor,
  '--bubble-base': props.baseColor,
  '--bubble-text': props.textColor,
  '--bubble-shadow': `0 10px 20px rgba(0, 0, 0, ${props.shadowIntensity})`,
  '--bubble-glow': `0 0 15px ${props.accentColor}66`, /* 40% opacidad */
}));
</script>

<style module>
.bubbleWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  user-select: none;
  transition: transform 0.2s ease;
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

.bubbleCheck {
  width: 1.6em;
  height: 1.6em;
  background: var(--bubble-base);
  /* El radio asimétrico base */
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border: 2px solid transparent;
}

/* Estado Marcado */
.checked .bubbleCheck {
  background: var(--bubble-accent);
  /* Cambia la forma orgánicamente */
  border-radius: 40% 60% 70% 30% / 40% 70% 30% 60%;
  box-shadow: var(--bubble-shadow), var(--bubble-glow);
  transform: scale(1.1) rotate(5deg);
}

.innerDot {
  width: 45%;
  height: 45%;
  background: white;
  border-radius: 50%;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
}

.label {
  font-weight: 700;
  color: var(--bubble-text);
  font-size: 0.9em;
}

/* Tamaños */
.sm { font-size: 14px; }
.md { font-size: 18px; }
.lg { font-size: 24px; }

/* Efecto Jelly al clickear */
.bubbleWrapper:active {
  transform: scale(0.9);
}

.bubbleWrapper:hover:not(.checked) .bubbleCheck {
  background: #d1d5db;
  transform: rotate(-10deg) scale(1.05);
}

/* Animación Jelly para el punto interno */
:global(.jelly-enter-active) {
  animation: jelly-in 0.5s;
}
@keyframes jelly-in {
  0% { transform: scale(0); }
  50% { transform: scale(1.4); }
  100% { transform: scale(1); }
}

@media (prefers-reduced-motion: reduce) {
  .bubbleCheck { transition: none; }
}
</style>