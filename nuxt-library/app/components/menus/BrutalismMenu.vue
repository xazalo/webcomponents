<template>
  <nav :class="$style.bentoMenu">
    <div :class="[$style.cell, $style.brandCell]">
      <span :class="$style.label">CURR:</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
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
          <span v-else :class="$style.square"></span>
        </div>
        <div :class="$style.contentArea">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
      </a>
    </div>

    <div :class="[$style.cell, $style.footerCell]">
      <button :class="$style.actionBtn">{{ buttonText }}</button>
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
  modelValue: string;
  links: NavLink[];
  buttonText: string;
}>();

defineEmits(['update:modelValue']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});
</script>

<style module>
:root {
  --nb-black: #000000;
  --nb-white: #ffffff;
  --nb-accent-1: #A2D2FF; /* Azul cielo */
  --nb-accent-2: #FFD6A5; /* Naranja pastel */
  --nb-accent-3: #CAFFBF; /* Verde lima */
  --nb-primary: #FFADAD; /* Rojo/Rosa fuerte */
  --nb-shadow: 6px 6px 0px #000000;
}

.bentoMenu {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 100%;
  max-width: 700px;
  padding: 16px;
  background: var(--nb-white);
  font-family: 'Arial Black', sans-serif;
}

.cell {
  background: var(--nb-white);
  border: 4px solid var(--nb-black);
  padding: 20px;
  position: relative;
  box-shadow: var(--nb-shadow);
  display: flex;
  flex-direction: column;
  transition: all 0.1s ease;
}

/* Celda de Título */
.brandCell {
  grid-column: span 3;
  background: var(--nb-primary);
}

.label {
  font-size: 0.7rem;
  font-weight: 900;
  color: var(--nb-black);
  text-transform: uppercase;
}

.title {
  color: var(--nb-black);
  margin: 5px 0 0;
  font-size: 2rem;
  text-transform: uppercase;
  line-height: 1;
}

/* Grid de Enlaces */
.linksGrid {
  grid-column: span 4;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.linkCell {
  text-decoration: none;
  cursor: pointer;
  background: var(--nb-accent-1);
}

/* Alternamos colores de celdas para el efecto bento caótico */
.linkCell:nth-child(2n) { background: var(--nb-accent-2); }
.linkCell:nth-child(3n) { background: var(--nb-accent-3); }

.linkCell:hover {
  transform: translate(-3px, -3px);
  box-shadow: 9px 9px 0px #000;
}

.linkCell:active {
  transform: translate(2px, 2px);
  box-shadow: 2px 2px 0px #000;
}

.active {
  background: var(--nb-black) !important;
}

.active .linkTitle, .active .linkDesc, .active .iconArea {
  color: var(--nb-white) !important;
}

.iconArea {
  margin-bottom: 12px;
  font-size: 1.5rem;
}

.square {
  display: block;
  width: 12px; height: 12px;
  background: var(--nb-black);
  border: 2px solid var(--nb-black);
}

.active .square {
  background: var(--nb-white);
}

.linkTitle {
  font-size: 1.2rem;
  font-weight: 900;
  color: var(--nb-black);
  display: block;
  line-height: 1.1;
  margin-bottom: 4px;
}

.linkDesc {
  font-size: 0.8rem;
  font-weight: 600;
  color: rgba(0,0,0,0.7);
  font-family: 'Arial', sans-serif;
}

/* Footer Cell */
.footerCell {
  grid-column: span 1;
  justify-content: center;
  align-items: center;
  background: #000;
  padding: 10px;
}

.actionBtn {
  background: transparent;
  border: none;
  color: var(--nb-white);
  font-family: inherit;
  font-weight: 900;
  font-size: 0.75rem;
  cursor: pointer;
}

@media (max-width: 550px) {
  .linksGrid { grid-template-columns: 1fr; }
  .brandCell { grid-column: span 4; }
  .footerCell { grid-column: span 4; }
}
</style>