<template>
  <div :class="$style.bentoPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">{{ label }}</span>
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
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
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
  label: string;
}>();

const emit = defineEmits(['update:modelValue']);

const prevPage = () => {
  if (props.modelValue > 1) emit('update:modelValue', props.modelValue - 1);
};

const nextPage = () => {
  if (props.modelValue < props.totalPages) emit('update:modelValue', props.modelValue + 1);
};

// Lógica para mostrar solo algunas páginas (ej: 1, 2, 3...)
const visiblePages = computed(() => {
  const pages = [];
  const start = Math.max(1, props.modelValue - 1);
  const end = Math.min(props.totalPages, start + 2);
  for (let i = start; i <= end; i++) pages.push(i);
  return pages;
});
</script>

<style module>
.bentoPagination {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: rgba(240, 240, 243, 0.5);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  width: fit-content;
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: #ffffff;
  padding: 8px 16px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  border: 1px solid rgba(0, 0, 0, 0.03);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
}

.navCell {
  padding: 8px 12px;
  cursor: pointer;
  color: #111827;
  transition: all 0.2s ease;
  justify-content: center;
}

.navCell:hover:not(:disabled) {
  background: #111827;
  color: #ffffff;
}

.navCell:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.statusCell {
  flex-direction: column;
  justify-content: center;
  gap: 2px;
  min-width: 80px;
}

.label {
  font-size: 0.6rem;
  font-weight: 800;
  color: #9ca3af;
  letter-spacing: 0.05em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1rem;
  font-weight: 800;
  color: #4f46e5; /* Color acento */
}

.separator, .total {
  font-size: 0.8rem;
  font-weight: 600;
  color: #d1d5db;
}

.quickJumpCell {
  gap: 6px;
  padding: 8px;
}

.pageBtn {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  border: none;
  background: transparent;
  font-size: 0.8rem;
  font-weight: 700;
  color: #6b7280;
  cursor: pointer;
  transition: all 0.2s ease;
}

.pageBtn:hover {
  background: #f3f4f6;
}

.activePage {
  background: #f3f4f6;
  color: #111827;
  box-shadow: inset 0 0 0 1px rgba(0,0,0,0.05);
}

@media (max-width: 400px) {
  .quickJumpCell { display: none; }
}
</style>