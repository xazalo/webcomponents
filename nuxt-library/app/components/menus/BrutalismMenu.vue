<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.bentoMenu, $style[size]]" 
    :style="neoStyles"
  >
    <div :class="[$style.cell, $style.brandCell]">
      <span :class="$style.label">{{ prefix }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div v-if="decorative" :class="$style.cornerDecoration">★</div>
    </div>

    <div :class="[$style.linksGrid, { [$style.stack]: stackOnMobile }]">
      <a 
        v-for="(link, index) in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          $style[`color-${(index % 3) + 1}`],
          { [$style.active]: modelValue === link.path }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.iconArea">
          <slot :name="`icon-${link.path}`">
            <component :is="link.icon" v-if="link.icon" />
            <div v-else :class="$style.square" />
          </slot>
        </div>
        
        <div :class="$style.contentArea">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">
            {{ link.description }}
          </span>
        </div>
      </a>
    </div>

    <div :class="[$style.cell, $style.footerCell]">
      <button :class="$style.actionBtn" @click="$emit('action')">
        {{ buttonText }}
      </button>
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
  icon?: any;
}

interface Props {
  modelValue: string;
  links: NavLink[];
  buttonText?: string;
  prefix?: string;
  // Estilo
  primaryColor?: string;   /* Color de la celda de título */
  accentColors?: string[]; /* Array de 3 colores para las celdas */
  shadowSize?: number;     /* En píxeles (ej: 6) */
  borderWidth?: number;    /* En píxeles (ej: 4) */
  size?: 'md' | 'lg';
  decorative?: boolean;    /* Muestra iconos extra de diseño */
  stackOnMobile?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'GO_PRO',
  prefix: 'STATUS:',
  primaryColor: '#FFADAD',
  accentColors: () => ['#A2D2FF', '#FFD6A5', '#CAFFBF'],
  shadowSize: 6,
  borderWidth: 4,
  size: 'md',
  decorative: true,
  stackOnMobile: true
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'NAV_NULL';
});

const neoStyles = computed(() => ({
  '--nb-primary': props.primaryColor,
  '--nb-acc-1': props.accentColors[0],
  '--nb-acc-2': props.accentColors[1],
  '--nb-acc-3': props.accentColors[2],
  '--nb-bw': `${props.borderWidth}px`,
  '--nb-ss': `${props.shadowSize}px`,
  '--nb-ss-hover': `${props.shadowSize + 3}px`,
}));
</script>

<style module>
.bentoMenu {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 100%;
  padding: 16px;
  background: #fff;
  font-family: 'Arial Black', 'Inter', sans-serif;
  --nb-black: #000;
}

.cell {
  background: #fff;
  border: var(--nb-bw) solid var(--nb-black);
  padding: 20px;
  position: relative;
  box-shadow: var(--nb-ss) var(--nb-ss) 0px var(--nb-black);
  display: flex;
  flex-direction: column;
  transition: all 0.15s cubic-bezier(0.18, 0.89, 0.32, 1.28);
}

/* --- Brand Cell --- */
.brandCell {
  grid-column: span 3;
  background: var(--nb-primary);
}

.label { font-size: 0.75rem; color: var(--nb-black); letter-spacing: 1px; }
.title { 
  margin: 4px 0 0; 
  font-size: 2.2rem; 
  text-transform: uppercase; 
  line-height: 0.9;
  letter-spacing: -1px;
}

/* --- Links Grid --- */
.linksGrid {
  grid-column: span 4;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.linkCell {
  text-decoration: none;
  cursor: pointer;
}

.color-1 { background: var(--nb-acc-1); }
.color-2 { background: var(--nb-acc-2); }
.color-3 { background: var(--nb-acc-3); }

.linkCell:hover {
  transform: translate(-3px, -3px);
  box-shadow: var(--nb-ss-hover) var(--nb-ss-hover) 0px var(--nb-black);
}

.active {
  background: var(--nb-black) !important;
  transform: translate(2px, 2px);
  box-shadow: 2px 2px 0px var(--nb-black);
}

.active .linkTitle, .active .linkDesc, .active .iconArea {
  color: #fff !important;
}

.iconArea { font-size: 1.8rem; margin-bottom: 12px; color: var(--nb-black); }
.square { width: 14px; height: 14px; background: var(--nb-black); border: 2px solid var(--nb-black); }
.active .square { background: #fff; border-color: #fff; }

.linkTitle { font-size: 1.3rem; font-weight: 900; line-height: 1; margin-bottom: 6px; color: var(--nb-black); }
.linkDesc { font-size: 0.85rem; font-family: 'Arial', sans-serif; color: rgba(0,0,0,0.8); font-weight: 600; }

/* --- Footer --- */
.footerCell {
  grid-column: span 1;
  background: var(--nb-black);
  justify-content: center;
  align-items: center;
  padding: 10px;
}

.actionBtn {
  background: transparent;
  border: none;
  color: #fff;
  font-family: inherit;
  font-weight: 900;
  text-transform: uppercase;
  cursor: pointer;
}

/* --- Decorativos --- */
.cornerDecoration {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 1.5rem;
}

/* --- Responsive --- */
@media (max-width: 600px) {
  .bentoMenu { grid-template-columns: 1fr; }
  .brandCell, .linksGrid, .footerCell { grid-column: span 1; }
  .stack { grid-template-columns: 1fr; }
}
</style>