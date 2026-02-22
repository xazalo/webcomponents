<template>
  <nav :class="$style.cyberMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <div :class="$style.scanline"></div>
      <span :class="$style.label">CORE_LINK_ESTABLISHED</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div :class="$style.cornerDeco"></div>
    </div>

    <div :class="$style.list">
      <a 
        v-for="(link, index) in links" 
        :key="link.path"
        :href="link.path"
        :class="[$style.cell, $style.linkCell, { [$style.active]: modelValue === link.path }]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.glitchOverlay"></div>
        <div :class="$style.content">
          <div :class="$style.topRow">
            <span :class="$style.index">ID_{{ (index + 1).toString().padStart(2, '0') }}</span>
            <span :class="$style.linkTitle">{{ link.label }}</span>
          </div>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        <div :class="$style.indicator">
          <div :class="$style.pixelSquare"></div>
        </div>
      </a>
    </div>

    <button :class="[$style.cell, $style.logoutBtn]">
      <span :class="$style.btnText">TERMINATE_SESSION</span>
      <span :class="$style.warning">!</span>
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

const props = defineProps<{
  modelValue: string;
  links: NavLink[];
}>();

defineEmits(['update:modelValue']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'OVERVIEW';
});
</script>

<style module>
:root {
  --cy-yellow: #fcee0a;
  --cy-blue: #00f0ff;
  --cy-pink: #ff003c;
  --cy-bg: #0d0d0f;
  --cy-gray: #1a1a1b;
}

.cyberMenu {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 12px;
  background: var(--cy-bg);
  width: 100%;
  max-width: 360px;
  border-right: 3px solid var(--cy-yellow);
}

.cell {
  background: var(--cy-gray);
  padding: 14px 18px;
  border: 1px solid #333;
  position: relative;
  overflow: hidden;
  /* El clásico corte diagonal Cyberpunk */
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%);
  transition: all 0.2s ease;
}

.headerCell {
  background: #000;
  border-left: 4px solid var(--cy-yellow);
}

.scanline {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(250, 238, 10, 0.05) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.label {
  font-family: 'Consolas', monospace;
  font-size: 0.6rem;
  font-weight: bold;
  color: var(--cy-yellow);
  letter-spacing: 2px;
}

.title {
  margin: 4px 0 0;
  font-size: 1.5rem;
  font-weight: 900;
  color: #fff;
  text-transform: uppercase;
  font-family: 'Arial Black', sans-serif;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.linkCell {
  text-decoration: none;
  cursor: pointer;
}

.linkCell:hover {
  background: #252528;
  border-color: var(--cy-blue);
  transform: translateX(4px);
}

.active {
  background: var(--cy-blue) !important;
  clip-path: polygon(10px 0, 100% 0, 100% 100%, 0 100%, 0 10px);
}

.active .linkTitle, .active .linkDesc, .active .index {
  color: #000 !important;
}

.topRow {
  display: flex;
  align-items: baseline;
  gap: 10px;
}

.index {
  font-family: monospace;
  font-size: 0.7rem;
  color: var(--cy-blue);
  font-weight: bold;
}

.linkTitle {
  font-weight: 900;
  font-size: 1.1rem;
  color: #fff;
  text-transform: uppercase;
}

.linkDesc {
  display: block;
  font-size: 0.65rem;
  color: #888;
  margin-top: 2px;
  letter-spacing: 0.5px;
}

.pixelSquare {
  width: 8px;
  height: 8px;
  background: var(--cy-blue);
  box-shadow: 0 0 10px var(--cy-blue);
}

.active .pixelSquare {
  background: #000;
  box-shadow: none;
}

.logoutBtn {
  margin-top: 10px;
  background: var(--cy-pink);
  border: none;
  color: #fff;
  font-weight: 900;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logoutBtn:hover {
  filter: brightness(1.2);
  box-shadow: 0 0 20px var(--cy-pink);
}

.warning {
  background: #000;
  color: var(--cy-pink);
  width: 18px;
  height: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  font-weight: bold;
}

.cornerDeco {
  position: absolute;
  top: 0; right: 0;
  width: 20px; height: 4px;
  background: var(--cy-yellow);
}
</style>