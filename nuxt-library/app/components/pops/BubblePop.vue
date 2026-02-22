<template>
  <Teleport to="body">
    <Transition name="pop">
      <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
        <div v-bind="$attrs" :class="$style.bubbleModal" :style="bubbleStyles">
          <div :class="$style.inner">
            <slot />
            <button @click="$emit('update:modelValue', false)" :class="$style.close">{{ buttonText }}</button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps<{ modelValue: boolean; color?: string; shadow?: string | boolean, buttonText: string }>();

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#10b981',
  '--b-shadow': props.shadow === true ? "0 20px 50px rgba(0,0,0,0.15)" : (props.shadow || 'none')
}));
</script>

<style module>
.overlay {
  position: fixed; inset: 0; background: rgba(255,255,255,0.4);
  backdrop-filter: blur(8px); display: flex; align-items: center; justify-content: center; z-index: 9999;
}

.bubbleModal {
  background: white;
  padding: 2.5rem;
  width: 90%;
  max-width: 400px;
  border-radius: 60% 40% 70% 30% / 40% 50% 60% 40%;
  border: 4px solid var(--b-color);
  box-shadow: var(--b-shadow);
  text-align: center;
}

.inner { display: flex; flex-direction: column; gap: 1.5rem; align-items: center; }

.close {
  background: var(--b-color); color: white; border: none;
  padding: 0.6rem 1.5rem; border-radius: 20px; font-weight: 700; cursor: pointer;
}
</style>