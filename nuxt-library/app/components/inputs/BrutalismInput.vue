<template>
  <div v-bind="$attrs" :class="$style.brutalRoot" :style="brutalStyles">
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

const brutalStyles = computed(() => ({
  '--b-accent': props.color || '#bc95ff', // Un púrpura vibrante por defecto
}));
</script>

<style module>
.brutalRoot {
  width: 100%;
}

.inputWrapper {
  width: 100%;
  position: relative;
}

.input {
  width: 100%;
  padding: 1rem 1.2rem;
  font-size: 1.1rem;
  font-weight: 800;
  color: #000000;
  background-color: #ffffff;
  
  /* Bordes Brutalistas */
  border: 3px solid #000000;
  border-radius: 4px; /* Casi cuadrado */
  
  /* Sombra sólida (Hard Shadow) */
  box-shadow: 6px 6px 0px 0px #000000;
  
  outline: none;
  transition: all 0.1s ease-out;
  appearance: none;
}

.input::placeholder {
  color: #666;
  text-transform: uppercase;
  font-size: 0.9rem;
  font-weight: 900;
  opacity: 0.8;
}

/* Estado Focus: El input se "activa" cambiando de color y posición */
.input:focus {
  background-color: var(--b-accent);
  transform: translate(-2px, -2px);
  box-shadow: 8px 8px 0px 0px #000000;
}

/* Al escribir, el fondo puede volverse blanco de nuevo si prefieres o quedarse con el acento */
.input:not(:placeholder-shown) {
  background-color: #ffffff;
}

/* Efecto de pulsado (aunque sea un input, se siente físico) */
.input:active {
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px 0px #000000;
}
</style>