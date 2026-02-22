<template>
  <label v-bind="$attrs" :class="$style.neuWrapper" :style="neuStyles">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.neuBox">
        <Icon v-if="modelValue" name="lucide:check" :class="$style.checkIcon" />
      </div>
    </div>
    <span v-if="label" :class="$style.label">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: boolean;
  label?: string;
  color?: string; // Color para el check interno
}>();

const neuStyles = computed(() => ({
  '--n-bg': '#e0e5ec', // Color base neumórfico
  '--n-accent': props.color || '#4f46e5',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.6)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.8)',
}));
</script>

<style module>
.neuWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.6rem 1.2rem;
  background: var(--n-bg);
  border-radius: 16px;
  user-select: none;
  transition: all 0.2s ease;
  
  /* Relieve suave para el contenedor */
  box-shadow: 
    4px 4px 8px var(--n-shadow-dark),
    -4px -4px 8px var(--n-shadow-light);
}

.neuWrapper:active {
  transform: scale(0.98);
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  display: none;
}

.neuBox {
  width: 1.5rem;
  height: 1.5rem;
  background: var(--n-bg);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  
  /* Box "saliente" por defecto */
  box-shadow: 
    3px 3px 6px var(--n-shadow-dark),
    -3px -3px 6px var(--n-shadow-light);
}

/* Estado Marcado: El box se "hunde" en la superficie */
.inputContainer input:checked ~ .neuBox {
  box-shadow: 
    inset 3px 3px 6px var(--n-shadow-dark),
    inset -3px -3px 6px var(--n-shadow-light);
}

.checkIcon {
  color: var(--n-accent);
  width: 1rem;
  height: 1rem;
  stroke-width: 4px;
  /* El check suele ser sutil para no romper el minimalismo */
  filter: drop-shadow(0 0 2px rgba(var(--n-accent), 0.2));
}

.label {
  font-size: 0.9rem;
  font-weight: 600;
  color: #6d7c90;
  letter-spacing: -0.01em;
}

/* Hover: Aumenta la profundidad del relieve */
.neuWrapper:hover .neuBox:not(input:checked ~ .neuBox) {
  box-shadow: 
    5px 5px 10px var(--n-shadow-dark),
    -5px -5px 10px var(--n-shadow-light);
}
</style>