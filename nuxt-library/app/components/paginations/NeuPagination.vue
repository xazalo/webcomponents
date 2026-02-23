<template>
  <div 
    v-bind="$attrs"
    :class="[$style.neuPagination, $style[size]]" 
    :style="neuStyles"
  >
    <button 
      :class="[$style.cell, $style.navBtn]" 
      @click="prevPage" 
      :disabled="modelValue === 1"
    >
      <slot name="prev-icon">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" :stroke="iconColor" stroke-width="3">
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
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" :stroke="iconColor" stroke-width="3">
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
  // Personalización Neumórfica
  baseColor?: string;     /* Color de la superficie (ej: #e0e5ec) */
  accentColor?: string;   /* Color de la página activa */
  iconColor?: string;     /* Color de las flechas */
  intensity?: number;     /* Fuerza de la sombra (0.1 a 0.5) */
  distance?: number;      /* Desplazamiento de sombra (2 a 12) */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  prefix: 'PAG_IN',
  baseColor: '#e0e5ec',
  accentColor: '#6d5dfc',
  iconColor: '#7a8ba9',
  intensity: 0.2,
  distance: 6,
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

const neuStyles = computed(() => {
  const dist = props.distance;
  const blur = dist * 2;
  // Cálculo de sombras basado en la baseColor (simplificado para el ejemplo)
  return {
    '--neu-bg': props.baseColor,
    '--neu-accent': props.accentColor,
    '--neu-text': props.iconColor,
    '--neu-shadow-dark': `rgba(163, 177, 198, ${props.intensity * 3})`,
    '--neu-shadow-light': `rgba(255, 255, 255, 0.8)`,
    '--neu-dist': `${dist}px`,
    '--neu-blur': `${blur}px`,
  };
});
</script>

<style module>
.neuPagination {
  display: flex;
  gap: 15px;
  padding: 15px;
  background: var(--neu-bg);
  border-radius: 40px;
  width: fit-content;
  align-items: center;
  box-shadow: var(--neu-dist) var(--neu-dist) var(--neu-blur) var(--neu-shadow-dark), 
             calc(var(--neu-dist) * -1) calc(var(--neu-dist) * -1) var(--neu-blur) var(--neu-shadow-light);
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: var(--neu-bg);
  border-radius: 20px;
  display: flex;
  box-shadow: calc(var(--neu-dist) / 1.5) calc(var(--neu-dist) / 1.5) var(--neu-blur) var(--neu-shadow-dark), 
             calc(var(--neu-dist) / -1.5) calc(var(--neu-dist) / -1.5) var(--neu-blur) var(--neu-shadow-light);
  transition: all 0.3s ease;
  border: none;
}

/* --- Botones --- */
.navBtn {
  width: 44px;
  height: 44px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.navBtn:active:not(:disabled) {
  box-shadow: inset 3px 3px 6px var(--neu-shadow-dark), 
              inset -3px -3px 6px var(--neu-shadow-light);
  transform: scale(0.97);
}

.navBtn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
  box-shadow: none;
  border: 1px solid rgba(0,0,0,0.05);
}

/* --- Status Cell --- */
.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 85px;
  padding: 8px 18px;
  gap: 2px;
}

.label {
  font-size: 0.6rem;
  font-weight: 800;
  color: var(--neu-text);
  letter-spacing: 0.05em;
  text-transform: uppercase;
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

/* --- Quick Jump (Hundido) --- */
.quickJumpCell {
  gap: 10px;
  padding: 8px 14px;
  box-shadow: inset 5px 5px 10px var(--neu-shadow-dark), 
              inset -5px -5px 10px var(--neu-shadow-light);
}

.pageBtn {
  width: 34px;
  height: 34px;
  border-radius: 12px;
  border: none;
  background: var(--neu-bg);
  font-size: 0.85rem;
  font-weight: 800;
  color: var(--neu-text);
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 3px 3px 6px var(--neu-shadow-dark), 
             -3px -3px 6px var(--neu-shadow-light);
}

.pageBtn:hover:not(.activePage) {
  transform: translateY(-1px);
}

.activePage {
  color: var(--neu-accent) !important;
  box-shadow: inset 3px 3px 6px var(--neu-shadow-dark), 
              inset -3px -3px 6px var(--neu-shadow-light);
}

/* --- Tamaños --- */
.sm { transform: scale(0.8); }
.lg { transform: scale(1.1); }

@media (max-width: 520px) {
  .quickJumpCell { display: none; }
}
</style>