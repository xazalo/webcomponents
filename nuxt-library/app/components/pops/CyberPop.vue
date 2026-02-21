<template>
  <Teleport to="body">
    <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
      <div v-bind="$attrs" :class="$style.modal" :style="modalStyles">
        <header :class="$style.header">
          <span :class="$style.title">{{ title }}</span>
          <button @click="$emit('update:modelValue', false)" :class="$style.closeBtn">×</button>
        </header>
        <div :class="$style.content">
          <slot />
        </div>
        <footer v-if="$slots.actions" :class="$style.footer">
          <slot name="actions" />
        </footer>
      </div>
    </div>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: boolean;
  title?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const modalStyles = computed(() => ({
  '--p-color': props.color || '#4f46e5',
  '--p-shadow': props.shadow === true ? "10px 10px 0px rgba(0,0,0,0.2)" : (props.shadow || 'none')
}));
</script>

<style module>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.modal {
  background: #111827;
  color: white;
  width: 90%;
  max-width: 450px;
  box-shadow: var(--p-shadow);
  clip-path: polygon(0 0, 95% 0, 100% 5%, 100% 100%, 5% 100%, 0 95%);
  border-top: 4px solid var(--p-color);
}

.header {
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.title { font-weight: 900; letter-spacing: 1px; text-transform: uppercase; }

.closeBtn {
  background: none; border: none; color: white; font-size: 1.5rem; cursor: pointer;
}

.content { padding: 1.5rem; line-height: 1.6; }

.footer {
  padding: 1rem;
  background: rgba(255,255,255,0.05);
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
}
</style>