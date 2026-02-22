<template>
  <Teleport to="body">
    <Transition name="bento-fade">
      <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
        <div v-bind="$attrs" :class="$style.bentoModal" :style="bentoStyles">
          <div :class="$style.bentoHeader">
            <div :class="$style.dot"></div>
            <div :class="$style.line"></div>
          </div>
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <button @click="$emit('update:modelValue', false)" :class="$style.closeAction">
              {{ buttonText }}
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps<{ modelValue: boolean; color?: string, buttonText: string }>();

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(0, 0, 0, 0.1); /* Fondo más oscuro y moderno */
  backdrop-filter: blur(12px); 
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
}

.bentoModal {
  background: #ffffff;
  padding: 1.5rem;
  width: 90%;
  max-width: 420px;
  border-radius: 1.5rem; /* Radio amplio y constante */
  border: 1px solid rgba(0, 0, 0, 0.08);
  box-shadow: 
    0 20px 40px -10px rgba(0, 0, 0, 0.1),
    0 0 0 1px rgba(0, 0, 0, 0.05);
  position: relative;
  overflow: hidden;
}

.bentoHeader {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 1.5rem;
  opacity: 0.3;
}

.dot { width: 8px; height: 8px; border-radius: 50%; background: var(--b-accent); }
.line { width: 40px; height: 4px; border-radius: 2px; background: #e5e7eb; }

.inner { display: flex; flex-direction: column; gap: 2rem; }

.content {
  font-family: system-ui, -apple-system, sans-serif;
  color: #374151;
  line-height: 1.5;
}

.closeAction {
  background: #f9fafb;
  color: #111827;
  border: 1px solid #e5e7eb;
  padding: 0.8rem 1.5rem;
  border-radius: 0.75rem;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s ease;
  width: 100%;
}

.closeAction:hover {
  background: #ffffff;
  border-color: var(--b-accent);
  color: var(--b-accent);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transform: translateY(-1px);
}

.closeAction:active {
  transform: translateY(0);
}
</style>