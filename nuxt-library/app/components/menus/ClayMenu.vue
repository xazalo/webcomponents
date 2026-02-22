<template>
  <nav :class="$style.clayMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <span :class="$style.label">NAV_</span>
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
        <div :class="$style.dot"></div>
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
  --clay-bg: #e2eafc;
  --clay-card: #ffffff;
  --clay-accent: #6c5ce7;
  --clay-pink: #ff7675;
  --clay-radius: 28px;
}

.clayMenu {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px;
  background: var(--clay-bg);
  width: 100%;
  max-width: 380px;
  border-radius: 40px;
}

.cell {
  background: var(--clay-card);
  border-radius: var(--clay-radius);
  padding: 20px 24px;
  border: none;
  position: relative;
  /* El alma del Claymorphism: Sombra exterior suave + sombras interiores para dar volumen mate */
  box-shadow: 
    12px 12px 24px rgba(0, 0, 0, 0.05),
    inset -6px -6px 12px rgba(0, 0, 0, 0.05),
    inset 6px 6px 12px rgba(255, 255, 255, 0.8);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.headerCell {
  background: var(--clay-card);
  margin-bottom: 8px;
}

.label {
  font-family: 'Quicksand', sans-serif;
  font-size: 0.65rem;
  font-weight: 700;
  color: #a29bfe;
  letter-spacing: 1px;
}

.title {
  margin: 4px 0 0;
  font-size: 1.6rem;
  font-weight: 800;
  color: #2d3436;
}

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
  transform: translateY(-4px);
  box-shadow: 
    16px 16px 32px rgba(0, 0, 0, 0.08),
    inset -6px -6px 12px rgba(0, 0, 0, 0.05),
    inset 6px 6px 12px rgba(255, 255, 255, 0.8);
}

.active {
  background: var(--clay-accent) !important;
  transform: scale(0.96);
  /* Sombra interna más fuerte cuando está activo */
  box-shadow: 
    inset 4px 4px 12px rgba(0, 0, 0, 0.15),
    inset -4px -4px 12px rgba(255, 255, 255, 0.2);
}

.active .linkTitle, .active .linkDesc {
  color: #ffffff !important;
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
}

.dot {
  width: 12px;
  height: 12px;
  background: var(--clay-accent);
  border-radius: 50%;
  box-shadow: inset 2px 2px 4px rgba(0,0,0,0.2);
}

.active .dot {
  background: #ffffff;
}

.logoutBtn {
  margin-top: 12px;
  background: var(--clay-pink);
  color: #fff;
  font-weight: 800;
  font-size: 0.9rem;
  cursor: pointer;
  text-align: center;
  /* Sombra interna para el botón de color */
  box-shadow: 
    8px 8px 16px rgba(255, 118, 117, 0.3),
    inset -6px -6px 12px rgba(0, 0, 0, 0.1),
    inset 6px 6px 12px rgba(255, 255, 255, 0.2);
}

.logoutBtn:active {
  transform: scale(0.95);
}
</style>