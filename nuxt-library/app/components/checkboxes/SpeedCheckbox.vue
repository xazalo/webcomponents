<template>
  <label v-bind="$attrs" :class="$style.speedWrapper" :style="speedStyles">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.speedBox">
        <Icon v-if="modelValue" name="lucide:zap" :class="$style.checkIcon" />
      </div>
    </div>
    <span v-if="label" :class="$style.label">{{ label }}</span>
    <div :class="$style.trail"></div>
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

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#ff004c',
  '--s-skew': '-15deg'
}));
</script>

<style module>
.speedWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.5rem 1.2rem;
  background: #0f0f0f;
  border-radius: 4px;
  position: relative;
  overflow: hidden;
  user-select: none;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  
  /* Inclinación aerodinámica del contenedor */
  transform: skewX(var(--s-skew));
  border-right: 4px solid transparent;
}

.speedWrapper:hover {
  background: #1a1a1a;
  transform: skewX(var(--s-skew)) translateX(5px);
  border-right-color: var(--s-accent);
}

.inputContainer {
  position: relative;
  display: flex;
  /* Corregimos inclinación para que el cuadro se vea recto */
  transform: skewX(calc(var(--s-skew) * -1));
}

.inputContainer input {
  display: none;
}

.speedBox {
  width: 1.3rem;
  height: 1.3rem;
  background: #2a2a2a;
  border: 1px solid #444;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

/* Estado Marcado */
.inputContainer input:checked ~ .speedBox {
  background: var(--s-accent);
  border-color: #fff;
  box-shadow: 0 0 15px var(--s-accent);
}

.checkIcon {
  color: white;
  width: 1rem;
  height: 1rem;
  stroke-width: 3px;
}

.label {
  position: relative;
  z-index: 2;
  font-size: 0.85rem;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  color: #eee;
  letter-spacing: 0.05em;
  /* Corregimos inclinación del texto */
  transform: skewX(calc(var(--s-skew) * -1));
  transition: color 0.3s ease;
}

/* Estela de velocidad decorativa */
.trail {
  position: absolute;
  top: 50%;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--s-accent), transparent);
  transition: left 0.4s ease;
  pointer-events: none;
}

.speedWrapper:hover .trail {
  left: 100%;
}

.speedWrapper:has(input:checked) .label {
  color: #fff;
  text-shadow: 0 0 8px var(--s-accent);
}

.speedWrapper:active {
  transform: skewX(var(--s-skew)) scale(0.95);
}
</style>