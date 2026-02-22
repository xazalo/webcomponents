<template>
  <div :class="$style.clayPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">PAG_</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.total">/ {{ totalPages }}</span>
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
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3">
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
  --clay-bg: #eef2f7;
  --clay-accent: #6366f1; /* Indigo Clay */
  --clay-radius: 20px;
  --clay-shadow-out: 8px 8px 16px rgba(0, 0, 0, 0.08), 
                    -8px -8px 16px rgba(255, 255, 255, 0.8);
  --clay-shadow-in: inset 4px 4px 8px rgba(0, 0, 0, 0.04), 
                   inset -4px -4px 8px rgba(255, 255, 255, 0.7);
}

.clayPagination {
  display: flex;
  gap: 12px;
  padding: 14px;
  background: var(--clay-bg);
  border-radius: calc(var(--clay-radius) * 1.5);
  box-shadow: var(--clay-shadow-out);
  width: fit-content;
  align-items: center;
}

.cell {
  background: var(--clay-bg);
  padding: 10px 18px;
  border-radius: var(--clay-radius);
  display: flex;
  box-shadow: var(--clay-shadow-out), var(--clay-shadow-in);
  border: none;
}

.navCell {
  padding: 10px;
  width: 46px;
  height: 46px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: #64748b;
  transition: all 0.3s ease;
}

.navCell:hover:not(:disabled) {
  transform: translateY(-2px);
  color: var(--clay-accent);
}

.navCell:active:not(:disabled) {
  transform: translateY(1px);
  box-shadow: inset 4px 4px 8px rgba(0, 0, 0, 0.1);
}

.navCell:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  box-shadow: var(--clay-shadow-in);
}

.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 85px;
  gap: 2px;
}

.label {
  font-family: 'Inter', sans-serif;
  font-size: 0.6rem;
  font-weight: 800;
  color: #94a3b8;
  letter-spacing: 0.05em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1.1rem;
  font-weight: 800;
  color: var(--clay-accent);
}

.total {
  font-size: 0.8rem;
  font-weight: 600;
  color: #cbd5e1;
}

.quickJumpCell {
  gap: 10px;
  padding: 10px 14px;
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: 12px;
  border: none;
  background: var(--clay-bg);
  font-size: 0.85rem;
  font-weight: 800;
  color: #64748b;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: var(--clay-shadow-out), var(--clay-shadow-in);
}

.activePage {
  background: var(--clay-accent);
  color: white !important;
  box-shadow: inset 4px 4px 10px rgba(0, 0, 0, 0.2), 
              4px 4px 12px rgba(99, 102, 241, 0.3);
  transform: scale(0.95);
}

@media (max-width: 500px) {
  .quickJumpCell { display: none; }
}
</style>