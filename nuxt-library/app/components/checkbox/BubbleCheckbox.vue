<template>
  <label v-bind="$attrs" :class="$style.bubbleWrapper">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
      />
      <div :class="$style.bubbleCheck" :style="bubbleStyles">
        <Icon v-if="modelValue" name="lucide:circle" :class="$style.dotIcon" />
      </div>
    </div>
    <span v-if="label">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: boolean;
  label?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#4f46e5',
  '--b-shadow': props.shadow === true ? "0 10px 15px -3px rgba(0, 0, 0, 0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.bubbleWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
}

.bubbleCheck {
  width: 1.6rem;
  height: 1.6rem;
  background: #f3f4f6;
  border: 2px solid #d1d5db;
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  display: flex;
  align-items: center;
  justify-content: center;
}

.inputContainer input { display: none; }

.inputContainer input:checked ~ .bubbleCheck {
  background: var(--b-color);
  border-color: var(--b-color);
  border-radius: 40% 60% 70% 30% / 40% 70% 30% 60%;
  box-shadow: var(--b-shadow);
  transform: scale(1.1);
}

.dotIcon {
  color: white;
  width: 0.8rem;
  fill: currentColor;
}
</style>