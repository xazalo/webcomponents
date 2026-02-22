<template>
  <div :class="$style.glassPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2.5">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">PAG</span>
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
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2.5">
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
.glassPagination {
  display: flex;
  gap: 10px;
  padding: 10px;
  /* El alma del Glassmorphism */
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px) saturate(160%);
  -webkit-backdrop-filter: blur(15px) saturate(160%);
  
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  width: fit-content;
  align-items: center;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.cell {
  background: rgba(255, 255, 255, 0.05);
  padding: 8px 16px;
  border-radius: 16px;
  display: flex;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.navCell {
  width: 42px;
  height: 42px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.08);
  transition: all 0.3s ease;
}

.navCell:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.4);
  transform: translateY(-2px);
}

.navCell:disabled {
  opacity: 0.2;
  cursor: not-allowed;
}

.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 80px;
  gap: 2px;
}

.label {
  font-family: 'Inter', sans-serif;
  font-size: 0.6rem;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.5);
  letter-spacing: 0.1em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1.1rem;
  font-weight: 700;
  color: #fff;
}

.total {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.3);
}

.quickJumpCell {
  gap: 8px;
  background: rgba(255, 255, 255, 0.03);
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  background: transparent;
  color: rgba(255, 255, 255, 0.6);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.pageBtn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.activePage {
  background: #fff !important;
  color: #000 !important;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
  border-color: #fff !important;
}

@media (max-width: 500px) {
  .quickJumpCell { display: none; }
}
</style>