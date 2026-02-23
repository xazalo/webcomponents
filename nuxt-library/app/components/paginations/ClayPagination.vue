<template>
  <div 
    v-bind="$attrs"
    :class="[$style.clayPagination, $style[size]]" 
    :style="clayStyles"
  >
    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3">
          <path d="m15 18-6-6 6-6"/>
        </svg>
      </slot>
    </button>

    <div v-if="showStatus" :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">{{ prefix }}</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.total">/ {{ totalPages }}</span>
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
    </div>

    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
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
  prefix?: string;
  // Configuración Clay
  clayColor?: string;     /* Color base (ej: #eef2f7) */
  accentColor?: string;   /* Color activo (ej: #6366f1) */
  depth?: number;         /* Intensidad del relieve (1-10) */
  borderRadius?: string;
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  prefix: 'PAG_',
  clayColor: '#eef2f7',
  accentColor: '#6366f1',
  depth: 6,
  borderRadius: '20px',
  size: 'md',
  showStatus: true,
  showNumbers: true
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
  const start = Math.max(1, props.modelValue - 1);
  const end = Math.min(props.totalPages, start + 2);
  for (let i = start; i <= end; i++) pages.push(i);
  return pages;
});

const clayStyles = computed(() => {
  const d = props.depth;
  return {
    '--c-bg': props.clayColor,
    '--c-accent': props.accentColor,
    '--c-radius': props.borderRadius,
    // Sombra exterior (Flotación)
    '--c-shadow-out': `${d}px ${d}px ${d*2}px rgba(0, 0, 0, 0.08)`,
    // Sombra interior (Volumen arcilloso)
    '--c-shadow-in': `inset ${d/2}px ${d/2}px ${d}px rgba(0, 0, 0, 0.04), 
                     inset -${d/2}px -${d/2}px ${d}px rgba(255, 255, 255, 0.7)`,
    // Brillo superior para efecto 3D
    '--c-highlight': `inset ${d/4}px ${d/4}px ${d/2}px rgba(255, 255, 255, 1)`,
  };
});
</script>

<style module>
.clayPagination {
  display: flex;
  gap: 12px;
  padding: 14px;
  background: var(--c-bg);
  border-radius: calc(var(--c-radius) * 1.5);
  box-shadow: var(--c-shadow-out);
  width: fit-content;
  align-items: center;
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: var(--c-bg);
  border-radius: var(--c-radius);
  display: flex;
  box-shadow: var(--c-shadow-out), var(--c-shadow-in), var(--c-highlight);
  border: none;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* --- Botones --- */
.navBtn {
  width: 44px;
  height: 44px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: #64748b;
}

.navBtn:hover:not(:disabled) {
  transform: translateY(-3px) scale(1.05);
  color: var(--c-accent);
}

.navBtn:active:not(:disabled) {
  transform: translateY(2px) scale(0.95);
  box-shadow: var(--c-shadow-in);
}

.navBtn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  box-shadow: var(--c-shadow-in);
}

/* --- Status --- */
.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 85px;
  padding: 8px 18px;
  gap: 1px;
}

.label {
  font-size: 0.55rem;
  font-weight: 800;
  color: #94a3b8;
  letter-spacing: 0.1em;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 3px;
}

.current {
  font-size: 1.1rem;
  font-weight: 800;
  color: var(--c-accent);
}

.total {
  font-size: 0.8rem;
  font-weight: 600;
  color: #cbd5e1;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 8px;
  padding: 8px 12px;
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: calc(var(--c-radius) * 0.6);
  border: none;
  background: var(--c-bg);
  font-size: 0.85rem;
  font-weight: 800;
  color: #64748b;
  cursor: pointer;
  box-shadow: var(--c-shadow-out), var(--c-shadow-in);
  transition: all 0.3s ease;
}

.activePage {
  background: var(--c-accent);
  color: white !important;
  transform: scale(0.92);
  box-shadow: 
    inset 4px 4px 8px rgba(0, 0, 0, 0.2),
    inset -4px -4px 8px rgba(255, 255, 255, 0.2),
    0 10px 20px rgba(0, 0, 0, 0.1);
}

/* --- Tamaños --- */
.sm { padding: 10px; gap: 8px; }
.sm .navBtn { width: 36px; height: 36px; }
.lg { padding: 20px; gap: 16px; }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>