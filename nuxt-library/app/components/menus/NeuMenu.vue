<template>
  <nav :class="$style.neuMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <span :class="$style.label">CONTROL_PANEL</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
    </div>

    <div :class="$style.list">
      <a 
        v-for="link in links" 
        :key="link.path"
        :href="link.path"
        :class="[$style.cell, $style.linkCell, { [$style.active]: modelValue === link.path }]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="[$style.statusIcon, { [$style.activeIcon]: modelValue === link.path }]"></div>
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
  /* El color base es CRÍTICO: no debe ser blanco puro ni negro puro */
  --neu-bg: #e0e5ec;
  --neu-light: #ffffff;
  --neu-shadow: #a3b1c6;
  --neu-accent: #6d5dfc;
}

.neuMenu {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 30px;
  background: var(--neu-bg);
  border-radius: 40px;
  width: 100%;
  max-width: 380px;
  /* Sombra del contenedor principal */
  box-shadow: 9px 9px 16px var(--neu-shadow), 
              -9px -9px 16px var(--neu-light);
}

.cell {
  background: var(--neu-bg);
  border-radius: 20px;
  padding: 18px 22px;
  border: none;
  transition: all 0.2s ease-in-out;
  position: relative;
}

/* Efecto de extrusión (hacia afuera) */
.headerCell {
  box-shadow: 6px 6px 12px var(--neu-shadow), 
              -6px -6px 12px var(--neu-light);
  margin-bottom: 10px;
}

.label {
  font-size: 0.65rem;
  font-weight: 800;
  color: #7a8ba9;
  letter-spacing: 1.5px;
}

.title {
  margin: 5px 0 0;
  font-size: 1.4rem;
  color: #44475a;
  font-weight: 700;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
  box-shadow: 4px 4px 8px var(--neu-shadow), 
              -4px -4px 8px var(--neu-light);
}

.linkCell:hover {
  transform: translateY(-2px);
  box-shadow: 6px 6px 12px var(--neu-shadow), 
              -6px -6px 12px var(--neu-light);
}

/* Efecto cóncavo (hacia adentro) al estar activo */
.active {
  box-shadow: inset 4px 4px 8px var(--neu-shadow), 
              inset -4px -4px 8px var(--neu-light) !important;
  transform: translateY(0);
}

.linkTitle {
  display: block;
  font-weight: 700;
  font-size: 1.05rem;
  color: #44475a;
}

.active .linkTitle {
  color: var(--neu-accent);
}

.linkDesc {
  font-size: 0.75rem;
  color: #7a8ba9;
}

.statusIcon {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--neu-bg);
  box-shadow: 2px 2px 4px var(--neu-shadow), 
              -2px -2px 4px var(--neu-light);
}

.activeIcon {
  background: var(--neu-accent);
  box-shadow: 0 0 10px rgba(109, 93, 252, 0.4);
}

.logoutBtn {
  margin-top: 15px;
  font-weight: 800;
  color: #ff7675;
  cursor: pointer;
  box-shadow: 4px 4px 8px var(--neu-shadow), 
              -4px -4px 8px var(--neu-light);
}

.logoutBtn:active {
  box-shadow: inset 4px 4px 8px var(--neu-shadow), 
              inset -4px -4px 8px var(--neu-light);
}
</style>