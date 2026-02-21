<template>
  <div v-bind="$attrs" :class="$style.bubbleRoot" :style="bubbleStyles">
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      :placeholder="placeholder"
      :class="$style.input"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: string | number;
  placeholder?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#4f46e5',
  '--b-shadow': props.shadow === true ? "0 10px 20px -5px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.bubbleRoot { width: 100%; }

.input {
  width: 100%;
  padding: 0.8rem 1.5rem;
  border: 2px solid #eee;
  background: #fafafa;
  border-radius: 20px 10px 20px 10px;
  outline: none;
  font-weight: 600;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: var(--b-shadow);
}

.input:focus {
  border-color: var(--b-color);
  border-radius: 10px 20px 10px 20px;
  background: white;
  transform: translateY(-2px);
}
</style>