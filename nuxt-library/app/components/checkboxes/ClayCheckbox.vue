<template>
  <label v-bind="$attrs" :class="$style.clayWrapper" :style="clayStyles">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.clayBox">
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

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#f472b6', // Rosa pastel por defecto
  '--c-bg': '#f0f4f8'
}));
</script>

<style module>
.clayWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.6rem 1.2rem;
  background: var(--c-bg);
  border-radius: 25px; /* Radio muy amplio */
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  user-select: none;
  
  /* Sombra exterior suave para flotación */
  box-shadow: 
    0 8px 16px rgba(0, 0, 0, 0.05),
    inset 0 2px 4px rgba(255, 255, 255, 0.8);
}

.clayWrapper:hover {
  transform: translateY(-2px);
  box-shadow: 
    0 12px 20px rgba(0, 0, 0, 0.08),
    inset 0 2px 4px rgba(255, 255, 255, 0.8);
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  display: none;
}

.clayBox {
  width: 1.6rem;
  height: 1.6rem;
  background: white;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  
  /* El secreto del Claymorphism: Sombras internas */
  box-shadow: 
    0 4px 8px rgba(0, 0, 0, 0.05),
    inset 0 2px 4px rgba(255, 255, 255, 1),
    inset 0 -3px 6px rgba(0, 0, 0, 0.05);
}

/* Estado Marcado */
.inputContainer input:checked ~ .clayBox {
  background: var(--c-accent);
  transform: scale(1.1) rotate(3deg);
  box-shadow: 
    0 6px 12px rgba(0, 0, 0, 0.1),
    inset 0 4px 6px rgba(255, 255, 255, 0.4),
    inset 0 -4px 8px rgba(0, 0, 0, 0.15);
}

.checkIcon {
  color: white;
  width: 1rem;
  height: 1rem;
  stroke-width: 4px;
  filter: drop-shadow(0 2px 2px rgba(0,0,0,0.1));
}

.label {
  font-size: 0.95rem;
  font-weight: 700;
  color: #64748b;
  letter-spacing: -0.01em;
}

/* Efecto al presionar */
.clayWrapper:active {
  transform: scale(0.95);
}
</style>