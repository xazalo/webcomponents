<template>
  <div 
    v-bind="$attrs"
    :class="[$style.neoPagination, $style[size]]" 
    :style="neoStyles"
  >
    <button 
      :class="[$style.cell, $style.navCell]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="4">
          <path d="m15 18-6-6 6-6"/>
        </svg>
      </slot>
    </button>

    <div v-if="showStatus" :class="[$style.cell, $style.statusCell]">
      <span :class="$style.label">{{ statusLabel }}</span>
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
    </div>

    <button 
      :class="[$style.cell, $style.navCell]" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
    >
      <slot name="next-icon">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="4">
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
  statusLabel?: string;
  // Personalización NeoBrutalista
  mainColor?: string;     /* Fondo del contenedor (ej: #FACC15) */
  accentColor?: string;   /* Color de selección (ej: #22d3ee) */
  borderWidth?: string;   /* Grosor de línea (ej: 3px) */
  shadowSize?: string;    /* Tamaño de la sombra dura (ej: 4px) */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  statusLabel: 'PAG_NUM',
  mainColor: '#FACC15',
  accentColor: '#22d3ee',
  borderWidth: '3px',
  shadowSize: '5px',
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

const neoStyles = computed(() => ({
  '--neo-main': props.mainColor,
  '--neo-accent': props.accentColor,
  '--neo-border-w': props.borderWidth,
  '--neo-shadow-s': props.shadowSize,
}));
</script>

<style module>
.neoPagination {
  display: flex;
  gap: 12px;
  padding: 12px;
  background: var(--neo-main);
  border: var(--neo-border-w) solid #000;
  box-shadow: var(--neo-shadow-s) var(--neo-shadow-s) 0px #000;
  width: fit-content;
  font-family: 'Arial Black', sans-serif;
}

.cell {
  background: #fff;
  padding: 6px 14px;
  border: var(--neo-border-w) solid #000;
  display: flex;
  align-items: center;
  transition: all 0.1s;
}

/* --- Botones de Navegación --- */
.navCell {
  cursor: pointer;
  justify-content: center;
  padding: 8px 12px;
}

.navCell:hover:not(:disabled) {
  background: var(--neo-accent);
  transform: translate(-3px, -3px);
  box-shadow: 3px 3px 0px #000;
}

.navCell:active:not(:disabled) {
  transform: translate(1px, 1px);
  box-shadow: none;
}

.navCell:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  background: #dfdfdf;
}

/* --- Celda de Status --- */
.statusCell {
  flex-direction: column;
  min-width: 90px;
  justify-content: center;
}

.label {
  font-size: 0.6rem;
  font-weight: 900;
  color: #000;
  letter-spacing: 1px;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.current {
  font-size: 1.3rem;
  color: #000;
}

.separator, .total {
  font-size: 0.9rem;
  color: #000;
  opacity: 0.6;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 8px;
  padding: 8px;
}

.pageBtn {
  width: 38px;
  height: 38px;
  border: var(--neo-border-w) solid #000;
  background: #fff;
  font-weight: 900;
  cursor: pointer;
  transition: all 0.1s;
}

.pageBtn:hover {
  background: #000;
  color: #fff;
}

.activePage {
  background: var(--neo-accent) !important;
  box-shadow: 3px 3px 0px #000;
  transform: translate(-3px, -3px);
}

/* --- Tamaños --- */
.sm { transform: scale(0.8); }
.lg { transform: scale(1.1); }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>