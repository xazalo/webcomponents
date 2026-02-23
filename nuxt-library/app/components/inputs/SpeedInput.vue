<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.speedRoot, $style[size], { [$style.disabled]: disabled }]" 
    :style="speedStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.inputWrapper">
      <div :class="$style.chassis" />
      
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="[$style.input, { [$style.hasContent]: !!modelValue }]"
      />
      
      <div :class="$style.nitroBar" />
      <div :class="$style.speedLines" />
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
  disabled?: boolean;
  // Personalización Speed
  accentColor?: string;   /* Color del flujo de energía */
  velocity?: number;      /* Ángulo de inclinación (5 a 25) */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'INPUT_SPEED_LOCK...',
  accentColor: '#00f2ff',
  velocity: 15,
  size: 'md'
});

defineEmits(['update:modelValue']);

const speedStyles = computed(() => ({
  '--s-accent': props.accentColor,
  '--s-skew': `-${props.velocity}deg`,
  '--s-skew-inv': `${props.velocity}deg`,
  '--s-glow': `${props.accentColor}44`,
}));
</script>

<style module>
.speedRoot {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  width: 100%;
}

.label {
  font-size: 0.75rem;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  color: #666;
  letter-spacing: 2px;
  padding-left: 1rem;
}

.inputWrapper {
  position: relative;
  width: 100%;
  transition: transform 0.2s ease;
}

.chassis {
  position: absolute;
  inset: 0;
  background: #111;
  transform: skewX(var(--s-skew));
  border-left: 4px solid #333;
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

.input {
  position: relative;
  width: 100%;
  background: transparent;
  border: none;
  outline: none;
  color: #fff;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  letter-spacing: 1px;
  z-index: 2;
  /* El texto se mantiene recto o con un sesgo menor para legibilidad */
  transform: skewX(0deg); 
  transition: all 0.3s ease;
}

/* Tamaños */
.sm .input { padding: 0.6rem 1rem; font-size: 0.9rem; }
.md .input { padding: 0.9rem 1.5rem; font-size: 1.1rem; }
.lg .input { padding: 1.2rem 2rem; font-size: 1.4rem; }

/* Estado Focus: "Full Throttle" */
.input:focus ~ .chassis {
  background: #1a1a1a;
  border-left-color: var(--s-accent);
  box-shadow: -10px 0 20px -5px var(--s-glow);
  transform: skewX(var(--s-skew)) translateX(5px);
}

.input:focus {
  padding-left: 2.5rem;
  color: var(--s-accent);
}

/* Nitro Bar */
.nitroBar {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0%;
  height: 3px;
  background: linear-gradient(90deg, var(--s-accent), transparent);
  transform: skewX(var(--s-skew));
  transition: width 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  z-index: 3;
}

.input:focus ~ .nitroBar {
  width: 100%;
}

/* Líneas de velocidad (decorativas) */
.speedLines {
  position: absolute;
  top: 50%;
  right: 10px;
  width: 40px;
  height: 2px;
  background: repeating-linear-gradient(90deg, var(--s-accent), var(--s-accent) 4px, transparent 4px, transparent 8px);
  opacity: 0;
  transform: translateY(-50%) skewX(var(--s-skew));
  transition: all 0.3s ease;
}

.input:focus ~ .speedLines {
  opacity: 0.6;
  right: -20px;
}

.input::placeholder {
  color: #444;
  font-style: normal;
  letter-spacing: 0;
}

.disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>