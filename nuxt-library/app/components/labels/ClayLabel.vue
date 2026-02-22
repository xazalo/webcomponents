<template>
  <span v-bind="$attrs" :class="$style.clayLabel" :style="clayStyles">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
}>();

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#a855f7', // Púrpura "candy" por defecto
}));
</script>

<style module>
.clayLabel {
  display: inline-flex;
  align-items: center;
  padding: 0.3rem 1rem;
  font-size: 0.75rem;
  font-weight: 800;
  color: white;
  background: var(--c-accent);
  cursor: default;
  user-select: none;
  white-space: nowrap;
  
  /* Radio muy redondeado tipo cápsula */
  border-radius: 12px;
  
  /* EL EFECTO CLAY: Sombra externa difusa + Brillo interno superior + Sombra interna inferior */
  box-shadow: 
    0 4px 10px rgba(0, 0, 0, 0.1),
    inset 0 3px 5px rgba(255, 255, 255, 0.4),
    inset 0 -3px 5px rgba(0, 0, 0, 0.2);
    
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.clayLabel:hover {
  transform: scale(1.1) rotate(2deg);
  box-shadow: 
    0 8px 15px rgba(0, 0, 0, 0.15),
    inset 0 4px 6px rgba(255, 255, 255, 0.5),
    inset 0 -4px 6px rgba(0, 0, 0, 0.2);
}

/* Un toque de brillo extra para que parezca plástico/goma */
.clayLabel::after {
  content: "";
  position: absolute;
  top: 15%;
  left: 15%;
  width: 20%;
  height: 20%;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  filter: blur(1px);
}
</style>