<template>
  <div v-bind="$attrs" :class="$style.glassRoot" :style="glassStyles">
    <div :class="$style.wrapper">
      <Icon v-if="icon" :name="icon" :class="$style.icon" />
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', event.target.value)"
        :placeholder="placeholder"
        :class="$style.input"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: string | number;
  placeholder?: string;
  icon?: string;
  shadow?: string | boolean;
}>();

const glassStyles = computed(() => ({
  '--g-shadow': props.shadow === true ? "0 8px 32px rgba(31, 38, 135, 0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.glassRoot { width: 100%; }

.wrapper {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.2rem 1rem;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 12px;
  box-shadow: var(--g-shadow);
  transition: border-color 0.3s;
}

.wrapper:focus-within {
  border-color: rgba(255, 255, 255, 0.8);
  background: rgba(255, 255, 255, 0.6);
}

.input {
  width: 100%;
  padding: 0.6rem 0;
  background: transparent;
  border: none;
  outline: none;
  color: #1f2937;
  font-weight: 500;
}

.icon {
  width: 1.2rem;
  color: #6b7280;
}
</style>