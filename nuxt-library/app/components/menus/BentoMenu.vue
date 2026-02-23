<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.bentoMenu, $style[size]]" 
    :style="navStyles"
  >
    <div :class="[$style.cell, $style.brandCell]">
      <div :class="$style.headerMeta">
        <span :class="$style.label">{{ prefix }}</span>
        <span :class="$style.version">v.{{ version }}</span>
      </div>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div :class="$style.glitchLine" />
      <div :class="$style.scanline" />
    </div>

    <div :class="[$style.linksGrid, { [$style.singleCol]: columns === 1 }]">
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
        <div :class="$style.iconArea">
          <slot :name="`icon-${link.path}`">
            <div :class="$style.defaultIcon" />
          </slot>
        </div>
        <div :class="$style.contentArea">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc" v-if="link.description">{{ link.description }}</span>
        </div>
        <div v-if="modelValue === link.path" :class="$style.activeIndicator" />
      </a>
    </div>

    <div :class="[$style.cell, $style.footerCell]">
      <button :class="$style.actionBtn" @click="$emit('action')">
        {{ buttonText }}
      </button>
      <div :class="$style.statusLight" />
    </div>
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
  version?: string;
  // Personalización
  accentColor?: string;   /* El amarillo Cyber o el Cyan */
  baseColor?: string;     /* Fondo de las celdas */
  columns?: 1 | 2;        /* Densidad del grid de enlaces */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'INITIALIZE',
  prefix: '// CORE //',
  version: '2.0.7',
  accentColor: '#fcee0a',
  baseColor: '#121213',
  columns: 2,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'SYSTEM_READY';
});

const navStyles = computed(() => ({
  '--c-accent': props.accentColor,
  '--c-accent-dim': `${props.accentColor}22`,
  '--c-bg-cell': props.baseColor,
  '--c-grid-cols': props.columns
}));
</script>

<style module>
.bentoMenu {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
  width: 100%;
  padding: 12px;
  background: #000;
  font-family: 'JetBrains Mono', 'Consolas', monospace;
  --c-blue: #00f0ff;
}

.cell {
  background: var(--c-bg-cell);
  border: 1px solid #2a2a2a;
  position: relative;
  overflow: hidden;
  transition: border-color 0.3s ease;
}

/* --- Brand Cell --- */
.brandCell {
  grid-column: span 3;
  padding: 20px;
  border-left: 5px solid var(--c-accent);
}

.headerMeta {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.label { font-size: 0.65rem; color: var(--c-accent); font-weight: 900; }
.version { font-size: 0.6rem; color: #444; }

.title {
  color: #fff;
  margin: 0;
  font-size: 1.8rem;
  text-transform: uppercase;
  letter-spacing: -1px;
}

/* --- Links Grid --- */
.linksGrid {
  grid-column: span 4;
  display: grid;
  grid-template-columns: repeat(var(--c-grid-cols), 1fr);
  gap: 12px;
}

.linkCell {
  padding: 16px;
  display: flex;
  gap: 15px;
  text-decoration: none;
  border: 1px solid #222;
}

.linkCell:hover {
  border-color: var(--c-blue);
  background: rgba(0, 240, 255, 0.03);
}

.active {
  background: var(--c-blue) !important;
  border-color: var(--c-blue);
}

.active .linkTitle { color: #000; }
.active .linkDesc { color: rgba(0,0,0,0.6); }

.iconArea { flex-shrink: 0; color: var(--c-blue); }
.active .iconArea { color: #000; }

.linkTitle { display: block; color: #fff; font-weight: 800; font-size: 0.95rem; }
.linkDesc { font-size: 0.7rem; color: #666; margin-top: 4px; display: block; }

/* --- Footer Cell --- */
.footerCell {
  grid-column: span 1;
  background: var(--c-accent);
  clip-path: polygon(0 0, 100% 0, 100% 65%, 65% 100%, 0 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 80px;
}

.actionBtn {
  background: transparent;
  border: none;
  font-weight: 950;
  font-size: 0.75rem;
  cursor: pointer;
  padding: 10px;
}

/* --- Efectos Decorativos --- */
.scanline {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(0,0,0,0.1) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.glitchLine {
  position: absolute;
  bottom: 0; left: 0; width: 60px; height: 3px;
  background: var(--c-accent);
  animation: glitch-slide 4s infinite;
}

@keyframes glitch-slide {
  0% { left: 0; width: 20px; }
  50% { left: 80%; width: 40px; }
  100% { left: 0; width: 20px; }
}

/* --- Responsividad --- */
@media (max-width: 600px) {
  .bentoMenu { grid-template-columns: 1fr; }
  .brandCell, .linksGrid, .footerCell { grid-column: span 1; }
  .linksGrid { grid-template-columns: 1fr; }
}
</style>