<template>
  <div 
    v-bind="$attrs"
    :class="[$style.glassPagination, $style[size]]" 
    :style="glassStyles"
  >
    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" :stroke="iconColor" stroke-width="2.5">
          <path d="m15 18-6-6 6-6"/>
        </svg>
      </slot>
    </button>

    <div v-if="showStatus" :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">{{ label }}</span>
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
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" :stroke="iconColor" stroke-width="2.5">
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
  // Personalización Glass
  glassColor?: string;    /* Color base (ej: 255, 255, 255) */
  blur?: string;          /* Intensidad del desenfoque (ej: 15px) */
  opacity?: number;       /* Opacidad del fondo (0 a 1) */
  accentColor?: string;   /* Color de la página activa */
  iconColor?: string;     /* Color de los SVGs */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  label: 'PAGE',
  glassColor: '255, 255, 255',
  blur: '15px',
  opacity: 0.1,
  accentColor: '#ffffff',
  iconColor: '#ffffff',
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

const glassStyles = computed(() => ({
  '--g-color': props.glassColor,
  '--g-blur': props.blur,
  '--g-opacity': props.opacity,
  '--g-accent': props.accentColor,
  '--g-text-accent': props.accentColor === '#ffffff' ? '#000000' : '#ffffff'
}));
</script>

<style module>
.glassPagination {
  display: flex;
  gap: 10px;
  padding: 10px;
  /* El motor del Glassmorphism */
  background: rgba(var(--g-color), var(--g-opacity));
  backdrop-filter: blur(var(--g-blur)) saturate(160%);
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(160%);
  
  border-radius: 24px;
  border: 1px solid rgba(var(--g-color), 0.2);
  width: fit-content;
  align-items: center;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: rgba(var(--g-color), 0.05);
  padding: 8px 16px;
  border-radius: 16px;
  display: flex;
  border: 1px solid rgba(var(--g-color), 0.1);
  transition: all 0.3s ease;
}

/* --- Botones de Navegación --- */
.navBtn {
  width: 42px;
  height: 42px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  background: rgba(var(--g-color), 0.08);
}

.navBtn:hover:not(:disabled) {
  background: rgba(var(--g-color), 0.2);
  border-color: rgba(var(--g-color), 0.4);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.navBtn:disabled {
  opacity: 0.2;
  cursor: not-allowed;
  transform: none;
}

/* --- Celda de Status --- */
.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 80px;
  gap: 2px;
}

.label {
  font-size: 0.6rem;
  font-weight: 800;
  color: rgba(var(--g-color), 0.5);
  letter-spacing: 0.12em;
  text-transform: uppercase;
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
  color: rgba(var(--g-color), 0.3);
  font-weight: 500;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 8px;
  background: rgba(var(--g-color), 0.03);
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: 10px;
  border: 1px solid rgba(var(--g-color), 0.1);
  background: transparent;
  color: rgba(var(--g-color), 0.6);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.pageBtn:hover {
  background: rgba(var(--g-color), 0.15);
  color: #fff;
}

.activePage {
  background: var(--g-accent) !important;
  color: var(--g-text-accent) !important;
  box-shadow: 0 0 25px rgba(var(--g-color), 0.4);
  border-color: var(--g-accent) !important;
  transform: scale(1.05);
}

/* --- Tamaños --- */
.sm { transform: scale(0.85); }
.lg { transform: scale(1.1); }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>