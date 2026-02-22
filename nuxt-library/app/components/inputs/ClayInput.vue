<template>
  <div v-bind="$attrs" :class="$style.clayRoot" :style="clayStyles">
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
  color?: string;
}>();

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#38bdf8', // Un azul cielo "blandito"
}));
</script>

<style module>
.clayRoot {
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
  font-weight: 700;
  color: #475569;
  background-color: #f0f4f8;
  border: none;
  outline: none;
  
  /* Radio extra redondeado */
  border-radius: 20px;
  
  /* EL EFECTO CLAY: Sombra externa + brillo interno + sombra interna inferior */
  box-shadow: 
    0 10px 20px -5px rgba(0, 0, 0, 0.05),
    inset 0 4px 6px rgba(255, 255, 255, 0.8),
    inset 0 -5px 10px rgba(0, 0, 0, 0.05);
    
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.input::placeholder {
  color: #94a3b8;
  font-weight: 500;
}

/* Estado Focus: El input se "infla" y resalta el color */
.input:focus {
  background-color: white;
  transform: scale(1.02) translateY(-2px);
  color: var(--c-accent);
  box-shadow: 
    0 15px 25px -8px rgba(0, 0, 0, 0.08),
    inset 0 4px 8px rgba(255, 255, 255, 1),
    inset 0 -2px 5px rgba(0, 0, 0, 0.03);
}

/* Toque final: un pequeño resplandor si tiene contenido */
.input:not(:placeholder-shown) {
  border: 2px solid rgba(255, 255, 255, 0.5);
}
</style>