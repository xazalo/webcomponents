<template>
  <div :class="$style.neoPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="4">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">PAG_NUM</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.separator">/</span>
        <span :class="$style.total">{{ totalPages }}</span>
      </div>
    </div>

    <div :class="[$style.cell, $style.quickJumpCell]">
      <button 
        v-for="page in visiblePages" 
        :key="page"
        :class="[$style.pageBtn, { [$style.activePage]: page === modelValue }]"
        @click="$emit('update:modelValue', page)"
      >
        {{ page }}
      </button>
    </div>

    <button 
      :class="$style.navCell" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="4">
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
  --neo-border: 3px solid #000;
  --neo-shadow: 4px 4px 0px #000;
  --color-main: #FACC15; /* Amarillo */
  --color-accent: #22d3ee; /* Cian */
}

.neoPagination {
  display: flex;
  gap: 12px;
  padding: 12px;
  background: var(--color-main);
  border: var(--neo-border);
  box-shadow: var(--neo-shadow);
  width: fit-content;
}

.cell {
  background: #fff;
  padding: 6px 14px;
  border: var(--neo-border);
  display: flex;
  align-items: center;
  transition: transform 0.1s;
}

.navCell {
  cursor: pointer;
  justify-content: center;
  padding: 6px 10px;
}

.navCell:hover:not(:disabled) {
  background: var(--color-accent);
  transform: translate(-2px, -2px);
  box-shadow: 2px 2px 0px #000;
}

.navCell:active:not(:disabled) {
  transform: translate(2px, 2px);
  box-shadow: none;
}

.navCell:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  background: #e5e7eb;
}

.statusCell {
  flex-direction: column;
  min-width: 90px;
  background: #fff;
}

.label {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.65rem;
  font-weight: 900;
  color: #000;
}

.counter {
  display: flex;
  align-items: center;
  gap: 4px;
  font-family: 'Courier New', Courier, monospace;
  font-weight: 900;
}

.current {
  font-size: 1.2rem;
  color: #000;
}

.separator, .total {
  font-size: 0.9rem;
  color: #666;
}

.quickJumpCell {
  gap: 8px;
  padding: 6px;
  background: #fff;
}

.pageBtn {
  width: 34px;
  height: 34px;
  border: 2px solid #000;
  background: #fff;
  font-family: 'Arial Black', sans-serif;
  font-weight: 900;
  cursor: pointer;
  transition: all 0.1s;
}

.pageBtn:hover {
  background: #eee;
  transform: scale(1.1);
}

.activePage {
  background: var(--color-accent) !important;
  box-shadow: 2px 2px 0px #000;
  transform: translate(-2px, -2px);
}

@media (max-width: 500px) {
  .quickJumpCell { display: none; }
}
</style>