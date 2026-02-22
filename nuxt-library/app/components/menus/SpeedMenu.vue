<template>
  <nav :class="$style.speedMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <div :class="$style.stripe"></div>
      <span :class="$style.label">{{ label }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
    </div>

    <div :class="$style.list">
      <a 
        v-for="(link, index) in links" 
        :key="link.path"
        :href="link.path"
        :class="[$style.cell, $style.linkCell, { [$style.active]: modelValue === link.path }]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.speedLine"></div>
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        <div :class="$style.rank">#0{{ index + 1 }}</div>
      </a>
    </div>

    <button :class="[$style.cell, $style.logoutBtn]">
      <span :class="$style.btnText">{{ buttonText }}</span>
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
  label?: string;
  buttonText: string;
}>();

defineEmits(['update:modelValue']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'DASHBOARD';
});
</script>

<style module>
:root {
  --speed-red: #ff3e3e;
  --speed-black: #121212;
  --speed-dark: #1a1a1a;
  --speed-white: #ffffff;
  --speed-accent: #00ff41; /* Verde nitro */
}

.speedMenu {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 20px;
  background: var(--speed-black);
  width: 100%;
  max-width: 400px;
  /* Textura sutil de fibra de carbono */
  background-image: 
    radial-gradient(circle at 2px 2px, rgba(255,255,255,0.05) 1px, transparent 0);
  background-size: 4px 4px;
}

.cell {
  background: var(--speed-dark);
  padding: 15px 25px;
  border: none;
  position: relative;
  /* Inclinación aerodinámica */
  transform: skewX(-10deg);
  transition: all 0.2s cubic-bezier(0.6, -0.28, 0.735, 0.045);
}

.headerCell {
  background: linear-gradient(90deg, var(--speed-red) 0%, #b30000 100%);
  border-left: 8px solid var(--speed-white);
  margin-bottom: 10px;
}

.stripe {
  position: absolute;
  top: 0; right: 20px; width: 40px; height: 100%;
  background: rgba(255,255,255,0.1);
  transform: skewX(20deg);
}

.label {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.7rem;
  font-style: italic;
  color: rgba(255,255,255,0.8);
  display: block;
}

.title {
  margin: 0;
  font-size: 1.8rem;
  font-weight: 900;
  font-style: italic;
  color: var(--speed-white);
  text-transform: uppercase;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  border-right: 4px solid transparent;
}

.linkCell:hover {
  background: #2a2a2a;
  transform: skewX(-10deg) translateX(10px);
  border-right-color: var(--speed-red);
}

.active {
  background: var(--speed-white) !important;
  transform: skewX(-10deg) scale(1.05);
  box-shadow: -10px 0 20px rgba(0,0,0,0.5);
}

.active .linkTitle, .active .linkDesc, .active .rank {
  color: var(--speed-black) !important;
}

.content {
  /* Anulamos el skew para que el texto sea legible */
  transform: skewX(10deg);
}

.linkTitle {
  display: block;
  font-weight: 900;
  font-size: 1.2rem;
  font-style: italic;
  color: var(--speed-white);
  text-transform: uppercase;
}

.linkDesc {
  font-size: 0.75rem;
  color: #888;
  font-weight: bold;
}

.rank {
  font-family: 'Courier New', monospace;
  font-weight: 900;
  color: var(--speed-red);
  transform: skewX(10deg);
}

.logoutBtn {
  margin-top: 15px;
  background: transparent;
  border: 2px solid var(--speed-red);
  color: var(--speed-red);
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
}

.logoutBtn:hover {
  background: var(--speed-red);
  color: var(--speed-white);
}

.logoutBtn .btnText {
  transform: skewX(10deg);
  display: inline-block;
}

/* Línea de velocidad animada al hacer hover */
.speedLine {
  position: absolute;
  bottom: 0; left: 0; width: 0; height: 2px;
  background: var(--speed-accent);
  transition: width 0.3s ease;
}

.linkCell:hover .speedLine {
  width: 100%;
}
</style>