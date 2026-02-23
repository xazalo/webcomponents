<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.glassRoot, $style[size], { [$style.disabled]: disabled }]" 
    :style="glassStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.wrapper">
      <div v-if="$slots.icon || icon" :class="$style.iconArea">
        <slot name="icon">
          <Icon :name="icon" />
        </slot>
      </div>

      <input
        :type="type"
        :value="modelValue"
        :disabled="disabled"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :class="$style.input"
      />

      <div :class="$style.specularHighlight" />
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
  icon?: string;
  disabled?: boolean;
  // Estética Glass
  blurAmount?: string;    /* Intensidad del desenfoque (ej: 12px) */
  tintColor?: string;     /* Color del tinte del cristal */
  opacity?: number;       /* Opacidad base (0 a 1) */
  borderRadius?: string;
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Escribe a través del cristal...',
  blurAmount: '12px',
  tintColor: '255, 255, 255', // Blanco por defecto (formato RGB)
  opacity: 0.2,
  borderRadius: '12px',
  size: 'md'
});

defineEmits(['update:modelValue']);

const glassStyles = computed(() => ({
  '--g-blur': props.blurAmount,
  '--g-bg': `rgba(${props.tintColor}, ${props.opacity})`,
  '--g-bg-focus': `rgba(${props.tintColor}, ${props.opacity + 0.15})`,
  '--g-border': `rgba(${props.tintColor}, 0.4)`,
  '--g-radius': props.borderRadius,
  '--g-text': props.opacity > 0.5 ? '#1f2937' : '#ffffff',
}));
</script>

<style module>
.glassRoot {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  width: 100%;
}

.label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #fff;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  margin-left: 0.2rem;
}

.wrapper {
  position: relative;
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0 1rem;
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur));
  -webkit-backdrop-filter: blur(var(--g-blur));
  border: 1px solid var(--g-border);
  border-radius: var(--g-radius);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 
    0 8px 32px 0 rgba(0, 0, 0, 0.1),
    inset 0 0 0 1px rgba(255, 255, 255, 0.1);
  overflow: hidden;
}

.wrapper:focus-within {
  background: var(--g-bg-focus);
  border-color: rgba(255, 255, 255, 0.6);
  transform: translateY(-1px);
  box-shadow: 
    0 12px 40px 0 rgba(0, 0, 0, 0.15),
    inset 0 0 0 1px rgba(255, 255, 255, 0.2);
}

.input {
  width: 100%;
  background: transparent;
  border: none;
  outline: none;
  color: var(--g-text);
  font-weight: 500;
  z-index: 2;
}

/* Tamaños */
.sm .input { padding: 0.5rem 0; font-size: 0.85rem; }
.md .input { padding: 0.8rem 0; font-size: 1rem; }
.lg .input { padding: 1.1rem 0; font-size: 1.2rem; }

.input::placeholder {
  color: rgba(255, 255, 255, 0.5);
}

.iconArea {
  display: flex;
  align-items: center;
  color: rgba(255, 255, 255, 0.7);
  z-index: 2;
}

/* Brillo especular superior (Efecto cristal premium) */
.specularHighlight {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 40%;
  background: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.15) 0%,
    rgba(255, 255, 255, 0) 100%
  );
  pointer-events: none;
  z-index: 1;
}

.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>