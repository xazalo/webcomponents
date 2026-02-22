<template>
  <div v-bind="$attrs" :class="$style.speedRoot" :style="speedStyles">
    <div :class="$style.inputWrapper">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :class="$style.input"
      />
      <div :class="$style.nitroBar"></div>
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

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#00f2ff', // Cyan eléctrico tipo neón
  '--s-skew': '-15deg',
}));
</script>

<style module>
.speedRoot {
  width: 100%;
}

.inputWrapper {
  width: 100%;
  position: relative;
  /* El contenedor lleva el skew para la forma general */
  transform: skewX(var(--s-skew));
}

.input {
  width: 100%;
  padding: 0.8rem 1.5rem;
  font-size: 1.1rem;
  font-weight: 900;
  font-style: italic; /* Esencial para el look racing */
  color: #ffffff;
  background-color: #111;
  border: none;
  border-left: 4px solid #333;
  outline: none;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Corregimos el texto para que no sea difícil de leer por el skew */
.input {
  transform: skewX(0deg); /* Opcional: puedes dejarlo sesgado para más agresividad */
}

.input::placeholder {
  color: #444;
  font-weight: 800;
}

/* Estado Focus: Activamos el motor */
.input:focus {
  background-color: #1a1a1a;
  border-left-color: var(--s-accent);
  padding-left: 2rem; /* El texto "se desplaza" por la aceleración */
  box-shadow: -10px 0 20px -10px var(--s-accent);
}

.nitroBar {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0%;
  height: 3px;
  background: linear-gradient(90deg, var(--s-accent), transparent);
  transition: width 0.4s ease;
}

.input:focus ~ .nitroBar {
  width: 100%;
}

/* Animación cuando hay texto: el borde izquierdo brilla */
.input:not(:placeholder-shown) {
  border-left-color: var(--s-accent);
}

/* Efecto de "vibración" sutil al escribir (opcional) */
.input:active {
  transform: translateX(2px);
}
</style>