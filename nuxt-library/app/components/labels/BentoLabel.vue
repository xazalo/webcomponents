<template>
  <span v-bind="$attrs" :class="$style.bentoLabel" :style="bentoStyles">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
  variant?: 'solid' | 'soft';
}>();

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoLabel {
  display: inline-flex;
  align-items: center;
  padding: 0.25rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: -0.01em;
  border-radius: 0.5rem; /* El radio estándar de las celdas Bento */
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: default;
  user-select: none;
  
  /* Look por defecto: 'soft' (estética Apple/Notion) */
  background: color-mix(in srgb, var(--b-accent) 12%, white);
  color: var(--b-accent);
  border: 1px solid color-mix(in srgb, var(--b-accent) 20%, transparent);
}

.bentoLabel:hover {
  transform: translateY(-1px);
  filter: brightness(0.95);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

/* Opcional: Variación sólida si se necesita más impacto */
/* .bentoLabelSolid {
  background: var(--b-accent);
  color: white;
  border: 1px solid rgba(0,0,0,0.1);
} 
*/
</style>