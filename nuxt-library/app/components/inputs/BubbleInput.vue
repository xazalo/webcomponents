<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.bubbleRoot, $style[size], { [$style.hasError]: error }]" 
    :style="bubbleStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.inputWrapper">
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="$style.input"
      />
      
      <div :class="$style.reflection" />
    </div>
    
    <span v-if="error && typeof error === 'string'" :class="$style.errorMessage">
      {{ error }}
    </span>
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
  disabled?: boolean;
  error?: boolean | string;
  // Personalización Bubble
  accentColor?: string;   /* Color del borde al enfocar */
  bubbleBg?: string;      /* Color de fondo */
  size?: 'sm' | 'md' | 'lg';
  roundness?: number;     /* Nivel de redondeo (0.5 a 2) */
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: '¡Hola! Escribe algo...',
  accentColor: '#4f46e5',
  bubbleBg: '#fafafa',
  size: 'md',
  roundness: 1
});

defineEmits(['update:modelValue']);

const bubbleStyles = computed(() => ({
  '--bb-accent': props.accentColor,
  '--bb-bg': props.bubbleBg,
  '--bb-radius-1': `${25 * props.roundness}px`,
  '--bb-radius-2': `${10 * props.roundness}px`,
  '--bb-glow': `${props.accentColor}33`,
}));
</script>

<style module>
.bubbleRoot {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  width: 100%;
}

.label {
  font-size: 0.9rem;
  font-weight: 800;
  color: #374151;
  padding-left: 0.5rem;
}

.inputWrapper {
  position: relative;
  width: 100%;
}

.input {
  width: 100%;
  background: var(--bb-bg);
  border: 3px solid #eee;
  /* Radio asimétrico inicial */
  border-radius: var(--bb-radius-1) var(--bb-radius-2) var(--bb-radius-1) var(--bb-radius-2);
  color: #1f2937;
  font-weight: 700;
  outline: none;
  transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* Tamaños */
.sm .input { padding: 0.6rem 1.2rem; font-size: 0.85rem; }
.md .input { padding: 0.9rem 1.5rem; font-size: 1rem; }
.lg .input { padding: 1.2rem 2rem; font-size: 1.2rem; }

/* Estado Focus: La burbuja cambia de forma e "infla" */
.input:focus {
  background: #ffffff;
  border-color: var(--bb-accent);
  /* Invierte el radio para el efecto de movimiento */
  border-radius: var(--bb-radius-2) var(--bb-radius-1) var(--bb-radius-2) var(--bb-radius-1);
  transform: scale(1.02) translateY(-2px);
  box-shadow: 0 15px 30px -10px var(--bb-glow);
}

/* Brillo decorativo superior */
.reflection {
  position: absolute;
  top: 8px;
  left: 15px;
  width: 20px;
  height: 6px;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 10px;
  pointer-events: none;
  transition: opacity 0.3s;
}

.input:focus ~ .reflection {
  opacity: 0.4;
}

/* Estado de Error con animación Shake */
.hasError .input {
  border-color: #f43f5e;
  background: #fff1f2;
  animation: shake 0.4s cubic-bezier(.36,.07,.19,.97) both;
}

.errorMessage {
  font-size: 0.8rem;
  font-weight: 700;
  color: #f43f5e;
  padding-left: 0.5rem;
}

@keyframes shake {
  10%, 90% { transform: translate3d(-1px, 0, 0); }
  20%, 80% { transform: translate3d(2px, 0, 0); }
  30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
  40%, 60% { transform: translate3d(4px, 0, 0); }
}

.input:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  filter: grayscale(1);
}
</style>