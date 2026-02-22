<template>
  <label v-bind="$attrs" :class="$style.brutalWrapper" :style="brutalStyles">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.brutalBox">
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
  color?: string;
}>();

const brutalStyles = computed(() => ({
  '--b-accent': props.color || '#00ff41', // Verde "Matrix" o similar funciona genial
}));
</script>

<style module>
.brutalWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  padding: 0.6rem 1rem;
  background: #ffffff;
  border: 3px solid #000000;
  /* El neo-brutalismo usa o 0 o un radio muy pequeño */
  border-radius: 4px; 
  box-shadow: 4px 4px 0px 0px #000000;
  transition: all 0.1s ease;
  user-select: none;
}

.brutalWrapper:hover {
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0px 0px #000000;
}

.brutalWrapper:active {
  transform: translate(2px, 2px);
  box-shadow: 0px 0px 0px 0px #000000;
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  display: none;
}

.brutalBox {
  width: 1.5rem;
  height: 1.5rem;
  background: #ffffff;
  border: 2px solid #000000;
  border-radius: 2px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.1s;
}

/* Estado Marcado */
.inputContainer input:checked ~ .brutalBox {
  background: var(--b-accent);
}

.checkIcon {
  color: #000000; /* Texto negro sobre fondo de color para máximo contraste */
  width: 1.1rem;
  height: 1.1rem;
  stroke-width: 4px;
}

.label {
  font-size: 1rem;
  font-weight: 900;
  text-transform: uppercase;
  color: #000000;
}

/* Efecto visual cuando está marcado el envoltorio */
.brutalWrapper:has(input:checked) {
  background: #f0f0f0;
}
</style>