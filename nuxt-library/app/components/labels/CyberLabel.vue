<template>
  <span v-bind="$attrs" :class="$style.cyberLabel" :style="labelStyles">
    <Icon v-if="icon" :name="icon" :class="$style.icon" />
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
  icon?: string;
  shadow?: string | boolean;
}>();

const labelStyles = computed(() => ({
  '--l-color': props.color || '#4f46e5',
  '--l-shadow': props.shadow === true ? "3px 3px 0px rgba(0,0,0,0.15)" : (props.shadow || 'none')
}));
</script>

<style module>
.cyberLabel {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.2rem 0.8rem;
  font-size: 0.7rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: white;
  background: var(--l-color);
  box-shadow: var(--l-shadow);
  clip-path: polygon(10% 0%, 100% 0%, 100% 70%, 90% 100%, 0% 100%, 0% 30%);
}

.icon {
  width: 0.9rem;
  height: 0.9rem;
}
</style>