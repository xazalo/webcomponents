<template>
  <span v-bind="$attrs" :class="$style.neuLabel" :style="neuStyles">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string; // Color del texto
}>();

const neuStyles = computed(() => ({
  '--n-bg': '#e0e5ec', // Gris azulado clásico
  '--n-text': props.color || '#6d7c90',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.7)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
}));
</script>

<style module>
.neuLabel {
  display: inline-flex;
  align-items: center;
  padding: 0.4rem 1.2rem;
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--n-text);
  background: var(--n-bg);
  cursor: default;
  user-select: none;
  white-space: nowrap;
  border-radius: 10px;
  transition: all 0.3s ease;
  
  /* RELIEVE NEUMÓRFICO: Luz arriba-izquierda, Sombra abajo-derecha */
  box-shadow: 
    5px 5px 10px var(--n-shadow-dark),
    -5px -5px 10px var(--n-shadow-light);
}

.neuLabel:hover {
  /* Al hover, se "eleva" un poco más aumentando el radio de la sombra */
  box-shadow: 
    7px 7px 14px var(--n-shadow-dark),
    -7px -7px 14px var(--n-shadow-light);
  transform: translateY(-1px);
}
</style>