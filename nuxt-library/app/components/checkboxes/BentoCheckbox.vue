<template>
  <label v-bind="$attrs" :class="$style.bentoWrapper" :style="bentoStyles">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.bentoBox">
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

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  padding: 0.5rem 0.8rem;
  background: #ffffff;
  border-radius: 1rem;
  border: 1px solid rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
  user-select: none;
}

.bentoWrapper:hover {
  background: #f9fafb;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  display: none;
}

.bentoBox {
  width: 1.4rem;
  height: 1.4rem;
  background: #f3f4f6;
  border-radius: 0.5rem; /* El clásico radio Bento */
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid rgba(0, 0, 0, 0.08);
}

/* Estado Marcado */
.inputContainer input:checked ~ .bentoBox {
  background: var(--b-accent);
  border-color: var(--b-accent);
  transform: scale(1.05);
  box-shadow: 0 4px 10px rgba(var(--b-accent), 0.3);
}

.checkIcon {
  color: white;
  width: 0.9rem;
  height: 0.9rem;
  stroke-width: 4px; /* Check grueso y legible */
}

.label {
  font-size: 0.9rem;
  font-weight: 600;
  color: #374151;
}

/* Efecto de pulsación al hacer click */
.bentoWrapper:active .bentoBox {
  transform: scale(0.92);
}
</style>