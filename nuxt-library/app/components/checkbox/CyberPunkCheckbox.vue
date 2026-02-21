<template>
  <label v-bind="$attrs" :class="$style.checkboxWrapper">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
        :disabled="disabled"
      />
      <div :class="$style.customCheck" :style="checkStyles">
        <Icon v-if="modelValue" name="lucide:check" :class="$style.checkIcon" />
      </div>
    </div>
    <span v-if="label" :class="$style.labelText">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';

defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label?: string;
  disabled?: boolean;
  color?: string;
  shadow?: string | boolean;
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
  color: '#4f46e5',
  shadow: true
});

defineEmits(['update:modelValue']);

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1)";

const checkStyles = computed(() => {
  let finalShadow = "none";
  if (typeof props.shadow === "string") finalShadow = props.shadow;
  else if (props.shadow === true) finalShadow = DEFAULT_SHADOW;

  return {
    '--check-color': props.color,
    '--check-shadow': finalShadow,
    '--check-opacity': props.disabled ? '0.5' : '1',
  };
});
</script>

<style module>
.checkboxWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  user-select: none;
}

.inputContainer {
  position: relative;
  width: 1.5rem;
  height: 1.5rem;
}

.inputContainer input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.customCheck {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: #e5e7eb;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: var(--check-opacity);
  /* Estilo Cyber */
  clip-path: polygon(20% 0%, 100% 0%, 100% 80%, 80% 100%, 0% 100%, 0% 20%);
}

.inputContainer input:checked ~ .customCheck {
  background-color: var(--check-color);
  box-shadow: var(--check-shadow);
}

.checkIcon {
  color: white;
  width: 1rem;
  height: 1rem;
  stroke-width: 3px;
}

.labelText {
  font-weight: 500;
  color: #374151;
}
</style>