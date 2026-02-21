<template>
  <label v-bind="$attrs" :class="$style.switch">
    <input 
      type="checkbox" 
      :checked="modelValue" 
      @change="$emit('update:modelValue', !modelValue)"
    >
    <span :class="$style.slider" :style="switchStyles"></span>
    <span v-if="label" :class="$style.label">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue?: boolean;
  label?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const switchStyles = computed(() => ({
  '--sw-color': props.color || '#10b981',
  '--sw-shadow': props.shadow ? '0 4px 6px rgba(0,0,0,0.15)' : 'none'
}));
</script>

<style module>
.switch {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.switch input { opacity: 0; width: 0; height: 0; }

.slider {
  width: 40px;
  height: 22px;
  background-color: #ccc;
  transition: .4s;
  border-radius: 34px;
  position: relative;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
}

.slider:before {
  content: "";
  position: absolute;
  height: 16px;
  width: 16px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
  box-shadow: var(--sw-shadow);
}

.switch input:checked ~ .slider {
  background-color: var(--sw-color);
}

.switch input:checked ~ .slider:before {
  transform: translateX(18px);
}
</style>