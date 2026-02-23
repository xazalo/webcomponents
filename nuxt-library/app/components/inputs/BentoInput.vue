<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.bentoRoot, $style[size]]" 
    :style="bentoStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    <div :class="$style.inputWrapper">
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="[$style.input, { [$style.hasIcon]: $slots.icon }]"
      />
      
      <div v-if="$slots.icon" :class="$style.iconArea">
        <slot name="icon" />
      </div>

      <div :class="$style.innerGlow" />
      <div :class="$style.focusRing" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: string | number;
  label?: string;
  placeholder?: string;
  type?: string;
  disabled?: boolean;
  // Personalización Bento
  accentColor?: string;
  borderRadius?: string;
  elevation?: 'flat' | 'low' | 'high';
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Escribe algo...',
  accentColor: '#4f46e5',
  borderRadius: '0.8rem',
  elevation: 'low',
  size: 'md'
});

defineEmits(['update:modelValue']);

const bentoStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-radius': props.borderRadius,
  '--b-shadow': props.elevation === 'high' 
    ? '0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04)' 
    : props.elevation === 'low' 
      ? '0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03)'
      : 'none',
}));
</script>

<style module>
.bentoRoot {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  width: 100%;
}

.label {
  font-size: 0.85rem;
  font-weight: 700;
  color: #4b5563;
  margin-left: 0.2rem;
  letter-spacing: -0.01em;
}

.inputWrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.input {
  width: 100%;
  background: #ffffff;
  border-radius: var(--b-radius);
  border: 1px solid rgba(0, 0, 0, 0.08);
  color: #1f2937;
  font-weight: 500;
  outline: none;
  box-shadow: var(--b-shadow);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1;
}

/* Tamaños */
.sm .input { padding: 0.6rem 0.9rem; font-size: 0.85rem; }
.md .input { padding: 0.9rem 1.2rem; font-size: 0.95rem; }
.lg .input { padding: 1.2rem 1.5rem; font-size: 1.1rem; }

.input::placeholder {
  color: #9ca3af;
  font-weight: 400;
}

/* Focus State: Bento Expansion */
.input:focus {
  border-color: var(--b-accent);
  transform: translateY(-2px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.08);
}

/* Soporte para Icono */
.hasIcon { padding-left: 3rem !important; }

.iconArea {
  position: absolute;
  left: 1rem;
  z-index: 2;
  display: flex;
  align-items: center;
  color: #9ca3af;
  pointer-events: none;
  transition: color 0.3s;
}

.input:focus ~ .iconArea {
  color: var(--b-accent);
}

/* El toque Bento: Reflejo superior interno */
.innerGlow {
  position: absolute;
  inset: 1px;
  border-radius: calc(var(--b-radius) - 1px);
  pointer-events: none;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.8);
  z-index: 2;
}

/* Anillo de enfoque sutil */
.focusRing {
  position: absolute;
  inset: -3px;
  border-radius: calc(var(--b-radius) + 3px);
  background: var(--b-accent);
  opacity: 0;
  transition: opacity 0.3s;
  z-index: 0;
}

.input:focus ~ .focusRing {
  opacity: 0.1;
}

.input:disabled {
  background: #f3f4f6;
  cursor: not-allowed;
  opacity: 0.7;
}
</style>