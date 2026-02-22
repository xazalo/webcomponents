<template>
  <nav :class="$style.glassMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <div :class="$style.shine"></div>
      <span :class="$style.label">_NAV</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
    </div>

    <div :class="$style.list">
      <a 
        v-for="(link) in links" 
        :key="link.path"
        :href="link.path"
        :class="[$style.cell, $style.linkCell, { [$style.active]: modelValue === link.path }]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        <div :class="$style.glassArrow">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <path d="M5 12h14M12 5l7 7-7 7"/>
          </svg>
        </div>
      </a>
    </div>

    <button :class="[$style.cell, $style.logoutBtn]">
      {{ buttonText }}
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
  buttonText: string;
}>();

defineEmits(['update:modelValue']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});
</script>

<style module>
:root {
  /* Fondo sugerido para que el efecto destaque: un gradiente mesh */
  --glass-border: rgba(255, 255, 255, 0.4);
  --glass-bg: rgba(255, 255, 255, 0.1);
  --glass-active: rgba(255, 255, 255, 0.25);
  --glass-radius: 20px;
}

.glassMenu {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 20px;
  /* Fondo oscuro semi-transparente para contener el efecto */
  background: rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(10px);
  border-radius: 30px;
  width: 100%;
  max-width: 380px;
  border: 1px solid var(--glass-border);
}

.cell {
  background: var(--glass-bg);
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  border-radius: var(--glass-radius);
  padding: 16px 20px;
  border: 1px solid var(--glass-border);
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.headerCell {
  background: rgba(255, 255, 255, 0.05);
  border-bottom: 2px solid var(--glass-border);
}

.shine {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(45deg, transparent, rgba(255,255,255,0.1), transparent);
  transform: rotate(45deg);
  pointer-events: none;
}

.label {
  font-size: 0.6rem;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 2px;
  text-transform: uppercase;
}

.title {
  margin: 2px 0 0;
  font-size: 1.5rem;
  font-weight: 300; /* Tipografía delgada clásica de glassmorphism */
  color: #ffffff;
  letter-spacing: -0.5px;
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
  cursor: pointer;
}

.linkCell:hover {
  background: var(--glass-active);
  transform: translateX(5px);
  border-color: #fff;
}

.active {
  background: rgba(255, 255, 255, 0.3) !important;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.2);
  border: 1px solid #fff;
}

.linkTitle {
  display: block;
  font-weight: 600;
  font-size: 1.1rem;
  color: #fff;
}

.linkDesc {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.5);
}

.glassArrow {
  color: #fff;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.linkCell:hover .glassArrow {
  opacity: 1;
}

.logoutBtn {
  margin-top: 8px;
  background: rgba(255, 71, 71, 0.2);
  border: 1px solid rgba(255, 71, 71, 0.4);
  color: #ff9f9f;
  font-weight: 700;
  font-size: 0.8rem;
  cursor: pointer;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.logoutBtn:hover {
  background: rgba(255, 71, 71, 0.4);
  color: #fff;
}
</style>