<template>
  <nav :class="$style.bentoMenu">
    <div :class="[$style.cell, $style.brandCell]">
      <span :class="$style.label">// CURR //</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div :class="$style.glitchLine"></div>
    </div>

    <div :class="$style.linksGrid">
      <a 
        v-for="link in links" 
        :key="link.path"
        :href="link.path"
        :class="[$style.cell, $style.linkCell, { [$style.active]: modelValue === link.path }]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.iconArea">
          <component :is="link.icon" v-if="link.icon" />
          <span v-else :class="$style.dot"></span>
        </div>
        <div :class="$style.contentArea">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
      </a>
    </div>

    <div :class="[$style.cell, $style.footerCell]">
      <button :class="$style.actionBtn">{{ buttonText }}</button>
      <div :class="$style.statusDot"></div>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface NavLink {
  label: string;
  path: string;
  description?: string;
  icon?: any;
}

const props = defineProps<{
  modelValue: string; // El path activo
  links: NavLink[];
  buttonText: string
}>();

defineEmits(['update:modelValue']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});
</script>

<style module>
:root {
  --cb-yellow: #fcee0a;
  --cb-blue: #00f0ff;
  --cb-bg: #050505;
  --cb-gray: #121213;
}

.bentoMenu {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto auto;
  gap: 10px;
  width: 100%;
  max-width: 600px;
  padding: 10px;
  background: var(--cb-bg);
  font-family: 'Consolas', monospace;
}

.cell {
  background: var(--cb-gray);
  border: 1px solid #2a2a2a;
  padding: 15px;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* Celda de Título */
.brandCell {
  grid-column: span 3;
  border-left: 4px solid var(--cb-yellow);
}

.label {
  font-size: 0.65rem;
  color: var(--cb-yellow);
  letter-spacing: 2px;
}

.title {
  color: #fff;
  margin: 5px 0 0;
  font-size: 1.5rem;
  text-transform: uppercase;
}

.glitchLine {
  position: absolute;
  bottom: 0; left: 0; width: 40%; height: 2px;
  background: var(--cb-yellow);
}

/* Grid de Enlaces */
.linksGrid {
  grid-column: span 4;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.linkCell {
  text-decoration: none;
  transition: all 0.2s ease;
  min-height: 80px;
  cursor: pointer;
}

.linkCell:hover {
  border-color: var(--cb-blue);
  background: rgba(0, 240, 255, 0.05);
  transform: translateY(-2px);
}

.active {
  background: var(--cb-blue) !important;
}

.active .linkTitle, .active .linkDesc {
  color: #000 !important;
}

.iconArea {
  margin-bottom: 10px;
  color: var(--cb-blue);
}

.dot {
  display: block;
  width: 6px; height: 6px;
  background: currentColor;
}

.linkTitle {
  font-weight: bold;
  color: #fff;
  display: block;
}

.linkDesc {
  font-size: 0.7rem;
  color: #666;
}

/* Footer Cell */
.footerCell {
  grid-column: span 1;
  justify-content: center;
  align-items: center;
  background: var(--cb-yellow);
  clip-path: polygon(0 0, 100% 0, 100% 70%, 70% 100%, 0 100%);
}

.actionBtn {
  background: transparent;
  border: none;
  font-weight: 900;
  font-size: 0.7rem;
  cursor: pointer;
}

@media (max-width: 480px) {
  .linksGrid { grid-template-columns: 1fr; }
  .brandCell { grid-column: span 4; }
  .footerCell { grid-column: span 4; height: 50px; }
}
</style>