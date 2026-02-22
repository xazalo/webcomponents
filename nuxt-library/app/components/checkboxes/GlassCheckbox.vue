<template>
  <label v-bind="$attrs" :class="$style.glassWrapper">
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', !modelValue)"
        :disabled="disabled"
      />
      <div :class="$style.glassCheck" :style="glassStyles">
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
  color?: string; // Color del check cuando está marcado
  blur?: string;
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
  color: '#ffffff',
  blur: '8px'
});

defineEmits(['update:modelValue']);

const glassStyles = computed(() => ({
  '--glass-blur': props.blur,
  '--glass-accent': props.color,
  '--glass-opacity': props.disabled ? '0.4' : '1',
}));
</script>

<style module>
.glassWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  user-select: none;
  padding: 0.4rem;
  border-radius: 0.8rem;
  transition: background 0.2s ease;
}

.glassWrapper:hover {
  background: rgba(255, 255, 255, 0.05);
}

.inputContainer {
  position: relative;
  width: 1.4rem;
  height: 1.4rem;
}

.inputContainer input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.glassCheck {
  position: absolute;
  inset: 0;
  /* El alma del Glassmorphism */
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(var(--glass-blur));
  -webkit-backdrop-filter: blur(var(--glass-blur));
  
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  opacity: var(--glass-opacity);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

/* Efecto al marcar */
.inputContainer input:checked ~ .glassCheck {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.5);
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.2);
}

.checkIcon {
  color: var(--glass-accent);
  width: 0.9rem;
  height: 0.9rem;
  stroke-width: 4px;
  filter: drop-shadow(0 0 2px rgba(0,0,0,0.3));
}

.labelText {
  font-weight: 500;
  color: white; /* Normalmente se usa sobre fondos oscuros/gradientes */
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  font-size: 0.95rem;
}

/* Animación de pulso al click */
.glassCheck:active {
  transform: scale(0.9);
}
</style>