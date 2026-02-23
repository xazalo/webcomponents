<template>
  <div 
    v-bind="$attrs"
    :class="[$style.speedPagination, $style[size]]" 
    :style="speedStyles"
  >
    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
          <path d="M11 17l-5-5 5-5M18 17l-5-5 5-5"/>
        </svg>
      </slot>
    </button>

    <div v-if="showStatus" :class="[$style.cell, $style.statusCell]">
      <div v-if="redline" :class="$style.redline"></div>
      <span :class="$style.label">{{ label }}</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue }}</span>
        <span :class="$style.total">/{{ totalPages }}</span>
      </div>
    </div>

    <div v-if="showNumbers" :class="[$style.cell, $style.quickJumpCell]">
      <button 
        v-for="page in visiblePages" 
        :key="page"
        :class="[$style.pageBtn, { [$style.activePage]: page === modelValue }]"
        @click="$emit('update:modelValue', page)"
      >
        <span :class="$style.btnText">{{ page }}</span>
      </button>
    </div>

    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
    >
      <slot name="next-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
          <path d="M13 17l5-5-5-5M6 17l5-5-5-5"/>
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
  // Configuración Racing
  teamColor?: string;     /* Color principal (ej: #e60000 - Ferrari Red) */
  skewAngle?: number;     /* Grados de inclinación (ej: -12) */
  texture?: boolean;      /* ¿Mostrar rejilla de fibra de carbono? */
  redline?: boolean;      /* Detalle de línea de límite de revoluciones */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  label: 'PAG / ID',
  teamColor: '#e60000',
  skewAngle: -12,
  texture: true,
  redline: true,
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

const speedStyles = computed(() => ({
  '--s-team': props.teamColor,
  '--s-skew': `skewX(${props.skewAngle}deg)`,
  '--s-unskew': `skewX(${Math.abs(props.skewAngle)}deg)`,
  '--s-grid': props.texture ? 'radial-gradient(#333 1px, transparent 0)' : 'none'
}));
</script>

<style module>
.speedPagination {
  display: flex;
  gap: 8px;
  padding: 10px;
  background: #0f0f11;
  background-image: var(--s-grid);
  background-size: 4px 4px;
  width: fit-content;
  align-items: center;
  border-bottom: 4px solid var(--s-team);
  font-family: 'Arial Black', sans-serif;
}

.cell {
  background: #1a1b1e;
  padding: 6px 16px;
  transform: var(--s-skew);
  border: 1px solid #333;
  display: flex;
  align-items: center;
  transition: all 0.15s ease-out;
}

/* --- Botones Navegación --- */
.navBtn {
  cursor: pointer;
  color: #fff;
  padding: 8px 12px;
}

.navBtn:hover:not(:disabled) {
  background: var(--s-team);
  border-color: #fff;
  transform: var(--s-skew) translateY(-2px);
}

.navBtn:disabled {
  opacity: 0.15;
  cursor: not-allowed;
  filter: grayscale(1);
}

/* --- Status Cell (Velocímetro) --- */
.statusCell {
  flex-direction: column;
  min-width: 110px;
  position: relative;
  border-left: 5px solid var(--s-team);
}

.redline {
  position: absolute;
  top: 0; right: 0; width: 30%; height: 3px;
  background: var(--s-team);
}

.label {
  font-size: 0.55rem;
  font-weight: 900;
  font-style: italic;
  color: #666;
  letter-spacing: 0.15em;
  transform: var(--s-unskew);
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 4px;
  transform: var(--s-unskew);
}

.current {
  font-size: 1.6rem;
  font-weight: 900;
  color: #fff;
  line-height: 1;
  font-style: italic;
}

.total {
  font-size: 0.8rem;
  color: var(--s-team);
  font-style: italic;
  opacity: 0.8;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 6px;
  background: transparent;
  border: none;
  padding: 0;
}

.pageBtn {
  width: 36px;
  height: 36px;
  transform: var(--s-skew);
  background: #2a2b2e;
  border: none;
  color: #888;
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btnText { transform: var(--s-unskew); }

.pageBtn:hover {
  background: #444;
  color: #fff;
}

.activePage {
  background: #fff !important;
  color: #000 !important;
  box-shadow: -5px 0 0 var(--s-team);
}

/* --- Tamaños --- */
.sm { transform: scale(0.8); }
.lg { transform: scale(1.15); }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>