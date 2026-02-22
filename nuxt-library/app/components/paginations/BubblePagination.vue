<template>
  <div :class="$style.bubblePagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">PAG</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.total">/ {{ totalPages }}</span>
      </div>
      <div :class="$style.glare"></div>
    </div>

    <div :class="[$style.cell, $style.quickJumpCell]">
      <button 
        v-for="page in visiblePages" 
        :key="page"
        :class="[$style.pageBtn, { [$style.activePage]: page === modelValue }]"
        @click="$emit('update:modelValue', page)"
      >
        {{ page }}
        <div v-if="page === modelValue" :class="$style.miniGlare"></div>
      </button>
    </div>

    <button 
      :class="$style.navCell" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
        <path d="m9 18 6-6-6-6"/>
      </svg>
    </button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  modelValue: number;
  totalPages: number;
}>();

const emit = defineEmits(['update:modelValue']);

const prevPage = () => {
  if (props.modelValue > 1) emit('update:modelValue', props.modelValue - 1);
};

const nextPage = () => {
  if (props.modelValue < props.totalPages) emit('update:modelValue', props.modelValue + 1);
};

const visiblePages = computed(() => {
  const pages = [];
  const start = Math.max(1, props.modelValue - 1);
  const end = Math.min(props.totalPages, start + 2);
  for (let i = start; i <= end; i++) pages.push(i);
  return pages;
});
</script>

<style module>
:root {
  --bubble-bg: #ffffff;
  --bubble-accent: #ff8dfb; /* Rosa chicle */
  --bubble-active-bg: #fdf2f8;
  --bubble-radius: 50px;
  --bubble-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  --bubble-inner: inset 2px 4px 8px rgba(255, 255, 255, 0.8), 
                  inset -2px -4px 8px rgba(0, 0, 0, 0.05);
}

.bubblePagination {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: var(--bubble-active-bg);
  border-radius: var(--bubble-radius);
  width: fit-content;
  box-shadow: var(--bubble-shadow);
  align-items: center;
}

.cell {
  background: var(--bubble-bg);
  padding: 10px 20px;
  border-radius: var(--bubble-radius);
  display: flex;
  position: relative;
  box-shadow: var(--bubble-inner);
  border: none;
  overflow: hidden;
}

.navCell {
  padding: 10px;
  width: 44px;
  height: 44px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: #ff70e0;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.navCell:hover:not(:disabled) {
  transform: scale(1.1);
  background: #fff;
  color: #ff8dfb;
}

.navCell:active:not(:disabled) {
  transform: scale(0.9);
}

.navCell:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 70px;
  gap: 2px;
}

.label {
  font-family: 'Rounded Mplus 1c', sans-serif;
  font-size: 0.6rem;
  font-weight: 900;
  color: #db96d3;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1.1rem;
  font-weight: 900;
  color: #ff70e0;
}

.total {
  font-size: 0.75rem;
  font-weight: 700;
  color: #e2e8f0;
}

.quickJumpCell {
  gap: 8px;
  padding: 8px 12px;
}

.pageBtn {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: none;
  background: transparent;
  font-size: 0.85rem;
  font-weight: 800;
  color: #94a3b8;
  cursor: pointer;
  position: relative;
  transition: all 0.3s ease;
}

.activePage {
  background: var(--bubble-accent);
  color: white !important;
  box-shadow: 0 4px 10px rgba(255, 141, 251, 0.4);
}

.glare {
  position: absolute;
  top: 4px;
  left: 15%;
  width: 70%;
  height: 4px;
  background: rgba(255, 255, 255, 0.4);
  border-radius: 10px;
  pointer-events: none;
}

.miniGlare {
  position: absolute;
  top: 4px;
  left: 8px;
  width: 6px;
  height: 4px;
  background: rgba(255, 255, 255, 0.4);
  border-radius: 50%;
}

@media (max-width: 450px) {
  .quickJumpCell { display: none; }
}
</style>