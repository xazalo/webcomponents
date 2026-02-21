<template>
  <span v-bind="$attrs" :class="$style.glassLabel" :style="glassStyles">
    <div :class="$style.dot" v-if="dot"></div>
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  dot?: boolean;
  shadow?: string | boolean;
}>();

const glassStyles = computed(() => ({
  '--g-shadow': props.shadow === true ? "0 4px 15px rgba(0,0,0,0.05)" : (props.shadow || 'none')
}));
</script>

<style module>
.glassLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.25rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 600;
  color: #1f2937;
  background: rgba(255, 255, 255, 0.45);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 6px;
  box-shadow: var(--g-shadow);
}

.dot {
  width: 6px;
  height: 6px;
  background: #f59e0b;
  border-radius: 50%;
  box-shadow: 0 0 8px #f59e0b;
}
</style>