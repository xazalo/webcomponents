<template>
  <div :class="$style.cyberPagination">
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
      <div :class="$style.scanline"></div>
      <span :class="$style.label">PAG_ID</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue.toString().padStart(2, '0') }}</span>
        <span :class="$style.total">/{{ totalPages.toString().padStart(2, '0') }}</span>
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
  --cb-bg: #050505;
  --cb-yellow: #fcee0a;
  --cb-blue: #00f0ff;
  --cb-pink: #ff003c;
  --cb-gray: #1a1a1b;
}

.cyberPagination {
  display: flex;
  gap: 8px;
  padding: 10px;
  background: var(--cb-bg);
  border: 1px solid var(--cb-yellow);
  clip-path: polygon(0 0, 98% 0, 100% 20%, 100% 100%, 2% 100%, 0 80%);
  width: fit-content;
  align-items: center;
}

.cell {
  background: var(--cb-gray);
  padding: 8px 16px;
  border: 1px solid #333;
  display: flex;
  position: relative;
  overflow: hidden;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 8px 100%, 0 calc(100% - 8px));
}

.scanline {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(0, 240, 255, 0.03) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.navCell {
  width: 40px;
  height: 40px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: var(--cb-yellow);
  transition: all 0.2s ease;
}

.navCell:hover:not(:disabled) {
  background: var(--cb-yellow);
  color: #000;
  box-shadow: 0 0 15px var(--cb-yellow);
}

.navCell:disabled {
  opacity: 0.2;
  cursor: not-allowed;
  filter: grayscale(1);
}

.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 90px;
  border-left: 3px solid var(--cb-blue);
}

.label {
  font-family: 'Consolas', monospace;
  font-size: 0.6rem;
  font-weight: bold;
  color: var(--cb-blue);
  letter-spacing: 1px;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 2px;
  font-family: 'Courier New', monospace;
}

.current {
  font-size: 1.2rem;
  font-weight: 900;
  color: #fff;
  text-shadow: 0 0 8px var(--cb-blue);
}

.total {
  font-size: 0.8rem;
  color: #555;
}

.quickJumpCell {
  gap: 6px;
  background: transparent;
  border: none;
}

.pageBtn {
  width: 32px;
  height: 32px;
  background: #111;
  border: 1px solid #444;
  color: #666;
  font-family: 'Consolas', monospace;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.2s;
}

.pageBtn:hover {
  border-color: var(--cb-pink);
  color: var(--cb-pink);
}

.activePage {
  background: var(--cb-pink) !important;
  color: #fff !important;
  border-color: var(--cb-pink) !important;
  box-shadow: 0 0 10px var(--cb-pink);
  animation: glitch 0.3s infinite;
}

@keyframes glitch {
  0% { transform: translate(0); }
  20% { transform: translate(-1px, 1px); }
  40% { transform: translate(-1px, -1px); }
  60% { transform: translate(1px, 1px); }
  80% { transform: translate(1px, -1px); }
  100% { transform: translate(0); }
}

@media (max-width: 500px) {
  .quickJumpCell { display: none; }
}
</style>