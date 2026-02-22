<template>
  <div :class="$style.speedPagination">
    <button 
      :class="$style.navCell" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
        <path d="M11 17l-5-5 5-5M18 17l-5-5 5-5"/>
      </svg>
    </button>

    <div :class="[$style.cell, $style.statusCell]">
      <div :class="$style.redline"></div>
      <span :class="$style.label">PAG / ID</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
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
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
        <path d="M13 17l5-5-5-5M6 17l5-5-5-5"/>
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
  --speed-black: #0f0f11;
  --speed-red: #e60000;
  --speed-white: #f0f0f0;
  --speed-skew: skewX(-12deg);
}

.speedPagination {
  display: flex;
  gap: 6px;
  padding: 8px;
  background: var(--speed-black);
  /* Textura de rejilla de competición */
  background-image: radial-gradient(#333 1px, transparent 0);
  background-size: 4px 4px;
  width: fit-content;
  align-items: center;
  border-bottom: 3px solid var(--speed-red);
}

.cell {
  background: #1a1b1e;
  padding: 6px 14px;
  transform: var(--speed-skew);
  border: 1px solid #333;
  display: flex;
  align-items: center;
  transition: all 0.2s ease;
}

.navCell {
  cursor: pointer;
  color: var(--speed-white);
  padding: 6px 10px;
}

.navCell:hover:not(:disabled) {
  background: var(--speed-red);
  border-color: var(--speed-red);
}

.navCell:disabled {
  opacity: 0.2;
  cursor: not-allowed;
  filter: grayscale(1);
}

.statusCell {
  flex-direction: column;
  min-width: 100px;
  position: relative;
  border-left: 4px solid var(--speed-red);
}

.redline {
  position: absolute;
  top: 0; right: 0; width: 20%; height: 2px;
  background: var(--speed-red);
}

.label {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.55rem;
  font-weight: 900;
  font-style: italic;
  color: #666;
  letter-spacing: 0.1em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 6px;
  transform: skewX(12deg); /* Des-inclinamos el texto interno para legibilidad */
}

.current {
  font-family: 'Orbitron', sans-serif; /* O una sans-serif muy bold */
  font-size: 1.4rem;
  font-weight: 900;
  color: var(--speed-white);
  line-height: 1;
}

.total {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.7rem;
  color: var(--speed-red);
  font-style: italic;
}

.quickJumpCell {
  gap: 4px;
  background: transparent;
  border: none;
}

.pageBtn {
  width: 32px;
  height: 32px;
  transform: var(--speed-skew);
  background: #2a2b2e;
  border: none;
  color: #999;
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
  transition: 0.1s;
}

.pageBtn:hover {
  background: #444;
  color: #fff;
}

.activePage {
  background: var(--speed-white) !important;
  color: var(--speed-black) !important;
  box-shadow: -4px 0 0 var(--speed-red);
}

@media (max-width: 500px) {
  .quickJumpCell { display: none; }
}
</style>