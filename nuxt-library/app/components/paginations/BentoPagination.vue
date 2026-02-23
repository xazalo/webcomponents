<template>
  <div 
    v-bind="$attrs"
    :class="[$style.bentoPagination, $style[size]]" 
    :style="bentoStyles"
  >
    <button 
      :class="[$style.cell, $style.navCell]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
      aria-label="Previous page"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3">
          <path d="m15 18-6-6 6-6"/>
        </svg>
      </slot>
    </button>

    <div v-if="showStatus" :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">{{ label }}</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.separator">/</span>
        <span :class="$style.total">{{ totalPages }}</span>
      </div>
    </div>

    <div v-if="showNumbers" :class="[$style.cell, $style.quickJumpCell]">
      <button 
        v-for="page in visiblePages" 
        :key="page"
        :class="[$style.pageBtn, { [$style.activePage]: page === modelValue }]"
        @click="$emit('update:modelValue', page)"
      >
        {{ page }}
      </button>
      
      <span v-if="totalPages > 3 && modelValue < totalPages - 1" :class="$style.dots">...</span>
    </div>

    <button 
      :class="[$style.cell, $style.navCell]" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
      aria-label="Next page"
    >
      <slot name="next-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3">
          <path d="m9 18 6-6-6-6"/>
        </svg>
      </slot>
    </button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: number;
  totalPages: number;
  label?: string;
  // Personalización Bento
  accentColor?: string;
  borderRadius?: string;
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
  maxVisible?: number;
}

const props = withDefaults(defineProps<Props>(), {
  label: 'PAGE',
  accentColor: '#4f46e5',
  borderRadius: '16px',
  size: 'md',
  showStatus: true,
  showNumbers: true,
  maxVisible: 3
});

const emit = defineEmits(['update:modelValue']);

const prevPage = () => {
  if (props.modelValue > 1) emit('update:modelValue', props.modelValue - 1);
};

const nextPage = () => {
  if (props.modelValue < props.totalPages) emit('update:modelValue', props.modelValue + 1);
};

const visiblePages = computed(() => {
  const pages = [];
  let start = Math.max(1, props.modelValue - Math.floor(props.maxVisible / 2));
  let end = Math.min(props.totalPages, start + props.maxVisible - 1);

  if (end - start + 1 < props.maxVisible) {
    start = Math.max(1, end - props.maxVisible + 1);
  }

  for (let i = start; i <= end; i++) pages.push(i);
  return pages;
});

const bentoStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-radius': props.borderRadius,
  '--b-bg-container': 'rgba(240, 240, 243, 0.6)',
}));
</script>

<style module>
.bentoPagination {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: var(--b-bg-container);
  backdrop-filter: blur(12px);
  border-radius: calc(var(--b-radius) + 8px);
  width: fit-content;
  font-family: 'Inter', system-ui, sans-serif;
  border: 1px solid rgba(255, 255, 255, 0.4);
}

.cell {
  background: #ffffff;
  border-radius: var(--b-radius);
  display: flex;
  align-items: center;
  border: 1px solid rgba(0, 0, 0, 0.04);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02), 0 2px 4px -1px rgba(0, 0, 0, 0.01);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

/* --- Nav Buttons --- */
.navCell {
  padding: 8px 12px;
  cursor: pointer;
  color: #1f2937;
  justify-content: center;
}

.navCell:hover:not(:disabled) {
  background: #111827;
  color: #ffffff;
  transform: translateY(-1px);
}

.navCell:active:not(:disabled) {
  transform: scale(0.95);
}

.navCell:disabled {
  opacity: 0.3;
  cursor: not-allowed;
  filter: grayscale(1);
}

/* --- Status Cell --- */
.statusCell {
  flex-direction: column;
  justify-content: center;
  gap: 1px;
  min-width: 70px;
  padding: 8px 20px;
}

.label {
  font-size: 0.55rem;
  font-weight: 800;
  color: #9ca3af;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1.1rem;
  font-weight: 900;
  color: var(--b-accent);
}

.separator, .total {
  font-size: 0.85rem;
  font-weight: 600;
  color: #d1d5db;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 4px;
  padding: 6px;
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: calc(var(--b-radius) - 6px);
  border: none;
  background: transparent;
  font-size: 0.85rem;
  font-weight: 700;
  color: #4b5563;
  cursor: pointer;
  transition: all 0.2s ease;
}

.pageBtn:hover {
  background: #f3f4f6;
  color: #111827;
}

.activePage {
  background: #f3f4f6;
  color: var(--b-accent) !important;
  box-shadow: inset 0 0 0 2px var(--b-accent);
}

.dots {
  color: #d1d5db;
  font-weight: 800;
  padding: 0 4px;
}

/* --- Sizes --- */
.sm { transform: scale(0.85); }
.lg { transform: scale(1.15); }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>