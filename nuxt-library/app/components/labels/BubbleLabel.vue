<template>
  <span v-bind="$attrs" :class="$style.bubbleLabel" :style="bubbleStyles">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
  shadow?: string | boolean;
}>();

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#10b981',
  '--b-shadow': props.shadow === true ? "0 4px 10px -2px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.bubbleLabel {
  display: inline-flex;
  padding: 0.3rem 1rem;
  font-size: 0.8rem;
  font-weight: 700;
  color: white;
  background: var(--b-color);
  box-shadow: var(--b-shadow);
  border-radius: 50px 20px 50px 20px;
  transition: transform 0.2s ease;
  cursor: default;
}

.bubbleLabel:hover {
  transform: rotate(-2deg) scale(1.05);
}
</style>