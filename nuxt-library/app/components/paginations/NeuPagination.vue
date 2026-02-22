<template>
  <div :class="$style.neuPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#7a8ba9" stroke-width="3">
        <path d="m15 18-6-6 6-6"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">PAG_IN</span>
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
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#7a8ba9" stroke-width="3">
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
  --neu-bg: #e0e5ec;
  --neu-light: #ffffff;
  --neu-shadow: #a3b1c6;
  --neu-accent: #6d5dfc;
  --neu-text: #7a8ba9;
}

.neuPagination {
  display: flex;
  gap: 15px;
  padding: 15px;
  background: var(--neu-bg);
  border-radius: 30px;
  width: fit-content;
  align-items: center;
  /* Sombra del contenedor principal */
  box-shadow: 9px 9px 16px var(--neu-shadow), 
             -9px -9px 16px var(--neu-light);
}

.cell {
  background: var(--neu-bg);
  padding: 10px 18px;
  border-radius: 20px;
  display: flex;
  /* Efecto de extrusión */
  box-shadow: 5px 5px 10px var(--neu-shadow), 
             -5px -5px 10px var(--neu-light);
}

.navCell {
  width: 46px;
  height: 46px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  border: none;
  outline: none;
  transition: all 0.2s ease;
}

.navCell:active:not(:disabled) {
  box-shadow: inset 3px 3px 6px var(--neu-shadow), 
              inset -3px -3px 6px var(--neu-light);
  transform: scale(0.98);
}

.navCell:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  box-shadow: none; /* Se aplana cuando no sirve */
  border: 1px solid rgba(255,255,255,0.3);
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
  color: var(--neu-text);
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
  color: #444;
}

.total {
  font-size: 0.8rem;
  color: var(--neu-text);
}

.quickJumpCell {
  gap: 8px;
  padding: 8px 12px;
  /* El contenedor de los botones de página está "hundido" */
  box-shadow: inset 5px 5px 10px var(--neu-shadow), 
              inset -5px -5px 10px var(--neu-light);
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: 12px;
  border: none;
  background: var(--neu-bg);
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--neu-text);
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 3px 3px 6px var(--neu-shadow), 
             -3px -3px 6px var(--neu-light);
}

.activePage {
  background: var(--neu-bg);
  color: var(--neu-accent) !important;
  /* El botón activo se "hunde" */
  box-shadow: inset 3px 3px 6px var(--neu-shadow), 
              inset -3px -3px 6px var(--neu-light);
}

@media (max-width: 520px) {
  .quickJumpCell { display: none; }
}
</style>