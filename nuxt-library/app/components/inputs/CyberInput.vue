<template>
  <div v-bind="$attrs" :class="$style.inputRoot" :style="inputStyles">
    <label v-if="label" :class="$style.label">{{ label }}</label>
    <div :class="$style.container">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :type="type"
        :disabled="disabled"
        :class="[$style.field, { [$style.hasError]: error }]"
      />
      <div :class="$style.accentLine"></div>
    </div>
    <span v-if="error" :class="$style.errorText">{{ error }}</span>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: string | number;
  label?: string;
  placeholder?: string;
  type?: string;
  error?: string;
  disabled?: boolean;
  color?: string;
  shadow?: string | boolean;
}>();

defineEmits(['update:modelValue']);

const inputStyles = computed(() => ({
  '--i-color': props.color || '#4f46e5',
  '--i-shadow': props.shadow === true ? "0 4px 15px rgba(79, 70, 229, 0.15)" : (props.shadow || 'none'),
  '--i-opacity': props.disabled ? '0.6' : '1'
}));
</script>

<style module>
.inputRoot {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  opacity: var(--i-opacity);
}

.label {
  font-size: 0.75rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #374151;
}

.container {
  position: relative;
  background: #f3f4f6;
  clip-path: polygon(10px 0%, 100% 0%, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0% 100%, 0% 10px);
}

.field {
  width: 100%;
  padding: 0.8rem 1.2rem;
  background: transparent;
  border: 2px solid #e5e7eb;
  outline: none;
  font-family: inherit;
  font-weight: 600;
  transition: all 0.3s ease;
  clip-path: polygon(10px 0%, 100% 0%, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0% 100%, 0% 10px);
}

.field:focus {
  border-color: var(--i-color);
  background: white;
  box-shadow: var(--i-shadow);
}

.hasError { border-color: #ef4444; }

.accentLine {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 3px;
  background: var(--i-color);
  transition: width 0.3s ease;
}

.field:focus + .accentLine { width: 40px; }

.errorText { font-size: 0.7rem; color: #ef4444; font-weight: 700; }
</style>