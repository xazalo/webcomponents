<template>
  <nav v-bind="$attrs" :class="$style.speedNav" :style="navStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.speedLink">
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
          <div :class="$style.shimmer"></div>
        </NuxtLink>
      </li>
    </ol>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface NavItem {
  label: string;
  to: string;
  icon?: string;
}

const props = defineProps<{
  items: NavItem[];
  color?: string; // Color de rastro/acento
}>();

const navStyles = computed(() => ({
  '--s-accent': props.color || '#0066ff',
  '--s-skew': '-15deg'
}));
</script>

<style module>
.speedNav {
  display: inline-block;
  padding: 1rem;
}

.list {
  display: flex;
  gap: 1.2rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  align-items: center;
}

.speedLink {
  position: relative;
  padding: 0.6rem 1.8rem;
  background: #0a0a0a;
  color: #ffffff;
  text-decoration: none;
  font-weight: 900;
  font-style: italic; /* Las itálicas aumentan la sensación de velocidad */
  font-size: 0.85rem;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  overflow: hidden;
  
  /* Inclinación aerodinámica */
  transform: skewX(var(--s-skew));
  border-left: 4px solid var(--s-accent);
  border-right: 0px solid transparent;
  
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  box-shadow: 6px 0 0 rgba(0, 0, 0, 0.2);
}

.speedLink:hover {
  background: #151515;
  color: var(--s-accent);
  transform: skewX(var(--s-skew)) translateX(8px); /* Se desplaza hacia adelante */
  border-right: 8px solid var(--s-accent);
  box-shadow: -10px 0 20px -5px rgba(var(--s-accent), 0.3);
}

.speedLink:hover .shimmer {
  animation: speedFlash 0.5s forwards;
}

.icon {
  font-size: 1.1rem;
  /* Corregimos la inclinación para el icono para que sea legible */
  transform: skewX(calc(var(--s-skew) * -1));
}

.label {
  /* Corregimos la inclinación para el texto */
  transform: skewX(calc(var(--s-skew) * -1));
  letter-spacing: 0.05em;
}

/* Efecto de destello al acelerar */
.shimmer {
  position: absolute;
  top: 0;
  left: -100%;
  width: 40%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transform: skewX(var(--s-skew));
}

@keyframes speedFlash {
  0% { left: -100%; }
  100% { left: 150%; }
}

/* Link activo: Estela de color persistente */
:global(.router-link-active) .speedLink {
  background: var(--s-accent);
  color: white;
  border-left-color: white;
  box-shadow: 0 0 15px var(--s-accent);
}

:global(.router-link-active) .label {
  text-shadow: 2px 2px 0px rgba(0,0,0,0.2);
}
</style>