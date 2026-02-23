<template>
  <div 
    v-bind="$attrs"
    :class="[$style.cyberPagination, $style[size]]" 
    :style="cyberStyles"
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
      <div v-if="scanlines" :class="$style.scanline"></div>
      <span :class="$style.label">{{ prefix }}</span>
      <div :class="$style.counter">
        <span :class="$style.current">{{ modelValue.toString().padStart(2, '0') }}</span>
        <span :class="$style.total">/{{ totalPages.toString().padStart(2, '0') }}</span>
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
  // Configuración Cyber
  primaryColor?: string;   /* Color de bordes y etiquetas (ej: #fcee0a) */
  accentColor?: string;    /* Color de selección/glitch (ej: #ff003c) */
  hudColor?: string;       /* Color de texto y HUD (ej: #00f0ff) */
  scanlines?: boolean;     /* Efecto de líneas CRT */
  glitchActive?: boolean;  /* ¿Animación de error en selección? */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;
  showNumbers?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  prefix: 'PAG_ID',
  primaryColor: '#fcee0a',
  accentColor: '#ff003c',
  hudColor: '#00f0ff',
  scanlines: true,
  glitchActive: true,
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

const cyberStyles = computed(() => ({
  '--cy-primary': props.primaryColor,
  '--cy-accent': props.accentColor,
  '--cy-hud': props.hudColor,
  '--cy-glitch-display': props.glitchActive ? 'glitch 0.3s infinite' : 'none'
}));
</script>

<style module>
.cyberPagination {
  display: flex;
  gap: 10px;
  padding: 12px;
  background: #050505;
  border: 1px solid var(--cy-primary);
  /* Corte angular cyber */
  clip-path: polygon(0 0, 95% 0, 100% 25%, 100% 100%, 5% 100%, 0 75%);
  width: fit-content;
  align-items: center;
  font-family: 'Consolas', 'Courier New', monospace;
}

.cell {
  background: #1a1a1b;
  padding: 8px 16px;
  border: 1px solid #333;
  display: flex;
  position: relative;
  overflow: hidden;
  /* Corte en la esquina inferior izquierda */
  clip-path: polygon(0 0, 100% 0, 100% 100%, 8px 100%, 0 calc(100% - 8px));
  transition: all 0.2s ease;
}

/* --- CRT Scanlines --- */
.scanline {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(0, 240, 255, 0.03) 50%);
  background-size: 100% 4px;
  pointer-events: none;
  z-index: 1;
}

/* --- Botones Navegación --- */
.navBtn {
  width: 42px;
  height: 42px;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: var(--cy-primary);
  border-color: #333;
}

.navBtn:hover:not(:disabled) {
  background: var(--cy-primary);
  color: #000;
  box-shadow: 0 0 15px var(--cy-primary);
  border-color: var(--cy-primary);
}

.navBtn:disabled {
  opacity: 0.15;
  cursor: not-allowed;
  filter: grayscale(1);
}

/* --- Status Cell --- */
.statusCell {
  flex-direction: column;
  align-items: center;
  min-width: 95px;
  border-left: 3px solid var(--cy-hud);
}

.label {
  font-size: 0.6rem;
  font-weight: bold;
  color: var(--cy-hud);
  letter-spacing: 2px;
  text-transform: uppercase;
}

.counter {
  display: flex;
  align-items: baseline;
  gap: 2px;
}

.current {
  font-size: 1.3rem;
  font-weight: 900;
  color: #fff;
  text-shadow: 0 0 10px var(--cy-hud);
}

.total {
  font-size: 0.8rem;
  color: #555;
  font-weight: bold;
}

/* --- Quick Jump --- */
.quickJumpCell {
  gap: 8px;
  background: transparent;
  border: none;
  padding: 0 5px;
  clip-path: none;
}

.pageBtn {
  width: 34px;
  height: 34px;
  background: #0a0a0a;
  border: 1px solid #444;
  color: #888;
  font-weight: bold;
  cursor: pointer;
  position: relative;
  transition: all 0.2s;
}

.btnText { position: relative; z-index: 2; }

.pageBtn:hover {
  border-color: var(--cy-accent);
  color: var(--cy-accent);
  box-shadow: inset 0 0 10px rgba(255, 0, 60, 0.1);
}

.activePage {
  background: var(--cy-accent) !important;
  color: #fff !important;
  border-color: var(--cy-accent) !important;
  box-shadow: 0 0 15px var(--cy-accent);
  animation: var(--cy-glitch-display);
}

/* --- Animations --- */
@keyframes glitch {
  0% { transform: translate(0); clip-path: inset(0 0 0 0); }
  20% { transform: translate(-2px, 2px); clip-path: inset(10% 0 40% 0); }
  40% { transform: translate(-2px, -2px); clip-path: inset(30% 0 20% 0); }
  60% { transform: translate(2px, 2px); clip-path: inset(50% 0 5% 0); }
  80% { transform: translate(2px, -2px); clip-path: inset(15% 0 60% 0); }
  100% { transform: translate(0); clip-path: inset(0 0 0 0); }
}

/* --- Tamaños --- */
.sm { transform: scale(0.8); }
.lg { transform: scale(1.15); }

@media (max-width: 500px) {
  .statusCell { display: none; }
}
</style>