<template>
  <span v-bind="$attrs" :class="$style.brutalLabel" :style="brutalStyles">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
}>();

const brutalStyles = computed(() => ({
  '--b-accent': props.color || '#ffff00', // Amarillo "alerta" por defecto
}));
</script>

<style module>
.brutalLabel {
  display: inline-flex;
  align-items: center;
  padding: 0.2rem 0.8rem;
  font-size: 0.8rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: #000000;
  background: var(--b-accent);
  
  /* Esencia Neo-Brutalista */
  border: 2px solid #000000;
  border-radius: 2px; /* Casi cuadrado para mayor rudeza */
  box-shadow: 3px 3px 0px 0px #000000;
  
  transition: all 0.1s ease;
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
}

.brutalLabel:hover {
  transform: translate(-1px, -1px);
  box-shadow: 4px 4px 0px 0px #000000;
}

.brutalLabel:active {
  transform: translate(2px, 2px);
  box-shadow: 0px 0px 0px 0px #000000;
}
</style>