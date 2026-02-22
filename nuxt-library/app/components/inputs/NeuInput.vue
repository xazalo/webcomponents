<template>
  <div v-bind="$attrs" :class="$style.neuRoot" :style="neuStyles">
    <div :class="$style.inputWrapper">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
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
  color?: string; // Color del texto al hacer focus
}>();

const neuStyles = computed(() => ({
  '--n-bg': '#e0e5ec', // Color base del neumorfismo
  '--n-accent': props.color || '#4f46e5',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.7)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
}));
</script>

<style module>
.neuRoot {
  width: 100%;
}

.inputWrapper {
  width: 100%;
  position: relative;
}

.input {
  width: 100%;
  padding: 1rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  color: #6d7c90;
  background-color: var(--n-bg);
  border: none;
  outline: none;
  border-radius: 12px;
  
  /* NEUMORPHISM: El input suele estar "hundido" (inset) por defecto */
  box-shadow: 
    inset 6px 6px 12px var(--n-shadow-dark),
    inset -6px -6px 12px var(--n-shadow-light);
    
  transition: all 0.3s ease;
}

.input::placeholder {
  color: #a0acba;
}

/* Estado Focus: El hundimiento se hace más profundo o el texto resalta */
.input:focus {
  box-shadow: 
    inset 8px 8px 16px var(--n-shadow-dark),
    inset -8px -8px 16px var(--n-shadow-light);
  color: var(--n-accent);
}

/* Opcional: Cuando el usuario no está escribiendo, puede verse hacia "afuera" */
/* .input:not(:focus) {
  box-shadow: 6px 6px 12px var(--n-shadow-dark), -6px -6px 12px var(--n-shadow-light);
} */
</style>