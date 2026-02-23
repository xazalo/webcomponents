<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.clayMenu, $style[size]]" 
    :style="clayStyles"
  >
    <div :class="[$style.cell, $style.headerCell]">
      <span :class="$style.label">{{ prefix }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div v-if="glossy" :class="$style.highlight" />
    </div>

    <div :class="$style.list">
      <a 
        v-for="link in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          { [$style.active]: modelValue === link.path }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="$style.dotContainer">
          <div :class="$style.dot" />
        </div>
      </a>
    </div>

    <button 
      v-if="buttonText"
      :class="[$style.cell, $style.actionBtn]" 
      @click="$emit('action')"
    >
      {{ buttonText }}
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

interface Props {
  modelValue: string;
  links: NavLink[];
  buttonText?: string;
  prefix?: string;
  // Personalización Clay
  themeColor?: string;    /* Color de la "arcilla" del menú */
  accentColor?: string;   /* Color del elemento activo */
  size?: 'sm' | 'md' | 'lg';
  depth?: number;         /* Intensidad del relieve (0.5 a 1.5) */
  borderRadius?: string;
  glossy?: boolean;       /* Añade un reflejo sutil en el header */
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '',
  prefix: 'NAV_',
  themeColor: '#e2eafc',
  accentColor: '#6c5ce7',
  size: 'md',
  depth: 1,
  borderRadius: '28px',
  glossy: true
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});

const clayStyles = computed(() => ({
  '--cl-bg': props.themeColor,
  '--cl-accent': props.accentColor,
  '--cl-radius': props.borderRadius,
  // Cálculo de profundidad dinámica
  '--cl-shadow-out': `${12 * props.depth}px ${12 * props.depth}px ${24 * props.depth}px rgba(0,0,0,0.06)`,
  '--cl-shadow-in-w': `inset ${6 * props.depth}px ${6 * props.depth}px ${12 * props.depth}px rgba(255,255,255,0.8)`,
  '--cl-shadow-in-d': `inset -${6 * props.depth}px -${6 * props.depth}px ${12 * props.depth}px rgba(0,0,0,0.05)`,
}));
</script>

<style module>
.clayMenu {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px;
  background: var(--cl-bg);
  width: 100%;
  max-width: 380px;
  border-radius: calc(var(--cl-radius) + 12px);
  font-family: 'Quicksand', 'Inter', sans-serif;
  box-shadow: var(--cl-shadow-out);
}

.cell {
  background: white;
  border-radius: var(--cl-radius);
  padding: 18px 24px;
  border: none;
  position: relative;
  overflow: hidden;
  box-shadow: var(--cl-shadow-in-w), var(--cl-shadow-in-d);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* --- Header --- */
.headerCell {
  background: #fff;
}

.label {
  font-size: 0.65rem;
  font-weight: 700;
  color: var(--cl-accent);
  letter-spacing: 1.5px;
  opacity: 0.7;
}

.title {
  margin: 4px 0 0;
  font-size: 1.6rem;
  font-weight: 800;
  color: #2d3436;
}

.highlight {
  position: absolute;
  top: 10%;
  left: 10%;
  width: 25%;
  height: 15%;
  background: rgba(255,255,255,0.5);
  border-radius: 50%;
  filter: blur(4px);
}

/* --- List --- */
.list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
}

.linkCell:hover {
  transform: translateY(-4px) scale(1.02);
  filter: brightness(1.02);
}

.active {
  background: var(--cl-accent) !important;
  transform: scale(0.96);
  box-shadow: 
    inset 4px 4px 10px rgba(0, 0, 0, 0.2),
    inset -4px -4px 10px rgba(255, 255, 255, 0.1);
}

.linkTitle {
  display: block;
  font-weight: 700;
  font-size: 1.1rem;
  color: #2d3436;
}

.linkDesc {
  font-size: 0.8rem;
  color: #636e72;
  font-weight: 500;
}

.active .linkTitle, .active .linkDesc {
  color: #fff !important;
}

/* --- Dot --- */
.dotContainer {
  width: 14px;
  height: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.dot {
  width: 100%;
  height: 100%;
  background: var(--cl-accent);
  border-radius: 50%;
  box-shadow: inset 2px 2px 4px rgba(0,0,0,0.15);
  transition: all 0.3s ease;
}

.active .dot {
  background: #fff;
  transform: scale(0.8);
}

/* --- Action Button --- */
.actionBtn {
  margin-top: 8px;
  background: #ff7675;
  color: #fff;
  font-weight: 800;
  cursor: pointer;
  text-align: center;
  box-shadow: 
    inset 6px 6px 12px rgba(255,255,255,0.2),
    inset -6px -6px 12px rgba(0,0,0,0.1);
}

.actionBtn:active {
  transform: scale(0.95);
}

/* --- Tamaños --- */
.sm { max-width: 320px; padding: 18px; }
.sm .cell { padding: 14px 20px; }
.lg { max-width: 450px; }
.lg .title { font-size: 2rem; }
</style>