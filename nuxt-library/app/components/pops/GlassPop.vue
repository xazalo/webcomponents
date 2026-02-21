<template>
  <Teleport to="body">
    <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
      <div v-bind="$attrs" :class="$style.glassCard" :style="glassStyles">
        <slot />
      </div>
    </div>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps<{ modelValue: boolean; shadow?: string | boolean }>();

const glassStyles = computed(() => ({
  '--g-shadow': props.shadow === true ? "0 25px 50px -12px rgba(0, 0, 0, 0.25)" : (props.shadow || 'none')
}));
</script>

<style module>
.overlay {
  position: fixed; inset: 0; background: rgba(0,0,0,0.2);
  display: flex; align-items: center; justify-content: center; z-index: 9999;
}

.glassCard {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.4);
  border-radius: 24px;
  padding: 2rem;
  width: 90%;
  max-width: 500px;
  box-shadow: var(--g-shadow);
}
</style>