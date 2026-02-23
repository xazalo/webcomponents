<template>
  <div 
    v-bind="$attrs"
    :class="[$style.bubblePagination, $style[size]]" 
    :style="bubbleStyles"
  >
    <button 
      :class="[$style.navBtn]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round">
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
      <div v-if="glare" :class="$style.glare"></div>
    </div>

    <div v-if="showNumbers" :class="[$style.cell, $style.quickJumpCell]">
      <button 
        v-for="page in visiblePages" 
        :key="page"
        :class="[$style.pageBtn, { [$style.activePage]: page === modelValue }]"
        @click="$emit('update:modelValue', page)"
      >
        {{ page }}
        <div v-if="glare && page === modelValue" :class="$style.miniGlare"></div>
      </button>
    </div>

    <button 
      :class="[$style.navBtn]" 
      @click="nextPage" 
      :disabled="modelValue === totalPages"
    >
      <slot name="next-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round">
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
  // Configuración Bubble
  accentColor?: string;   /* Color del "líquido" activo (ej: #ff8dfb) */
  baseColor?: string;     /* Color de las burbujas fondo (ej: #ffffff) */
  glare?: boolean;        /* Destellos de luz */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  label: 'PAG',
  accentColor: '#ff8dfb',
  baseColor: '#ffffff',
  glare: true,
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

const bubbleStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-bg': props.baseColor,
  '--b-glow': `${props.accentColor}44`,
}));
</script>

<style module>
.bubblePagination {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
  border-radius: 100px;
  width: fit-content;
  align-items: center;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

.cell {
  background: var(--b-bg);
  padding: 10px 22px;
  border-radius: 100px;
  display: flex;
  position: relative;
  /* Sombra interna para efecto 3D soft */
  box-shadow: 
    inset 2px 4px 8px rgba(255, 255, 255, 0.8), 
    inset -2px -4px 8px rgba(0, 0, 0, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.5);
}

/* --- Botones de Dirección --- */
.navBtn {
  width: 46px;
  height: 46px;
  border-radius: 50%;
  border: none;
  background: var(--b-bg);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: var(--b-accent);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.navBtn:hover:not(:disabled) {
  transform: scale(1.15) rotate(5deg);
  background: #fff;
}

.navBtn:active:not(:disabled) {
  transform: scale(0.9);
}

.navBtn:disabled {
  opacity: 0.3;
  filter: grayscale(1);
  cursor: not-allowed;
}

/* --- Status Cell --- */
.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 75px;
  gap: 1px;
}

.label {
  font-size: 0.6rem;
  font-weight: 900;
  color: #b0b0b0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 3px;
}

.current {
  font-size: 1.2rem;
  font-weight: 900;
  color: var(--b-accent);
}

.total {
  font-size: 0.8rem;
  font-weight: 700;
  color: #d1d5db;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 8px;
  padding: 8px 14px;
}

.pageBtn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: none;
  background: transparent;
  font-size: 0.9rem;
  font-weight: 900;
  color: #94a3b8;
  cursor: pointer;
  position: relative;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.pageBtn:hover {
  background: rgba(0, 0, 0, 0.03);
  color: var(--b-accent);
}

.activePage {
  background: var(--b-accent) !important;
  color: white !important;
  box-shadow: 0 8px 20px var(--b-glow);
  transform: scale(1.1);
}

/* --- Efectos de Brillo (Efecto Cristal/Caramelo) --- */
.glare {
  position: absolute;
  top: 4px;
  left: 20%;
  width: 60%;
  height: 5px;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.8), transparent);
  border-radius: 10px;
  pointer-events: none;
}

.miniGlare {
  position: absolute;
  top: 4px;
  left: 50%;
  transform: translateX(-50%);
  width: 10px;
  height: 4px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
}

/* --- Tamaños --- */
.sm { transform: scale(0.8); }
.lg { transform: scale(1.1); }

@media (max-width: 450px) {
  .quickJumpCell { display: none; }
}
</style>