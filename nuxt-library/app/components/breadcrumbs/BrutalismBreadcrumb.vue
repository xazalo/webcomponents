<template>
  <nav v-bind="$attrs" :class="$style.brutalNav" :style="navStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.brutalLink">
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
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
  color?: string; // Color para el estado hover/activo
}>();

const navStyles = computed(() => ({
  '--b-accent': props.color || '#bc95ff', // Un púrpura vibrante por defecto
}));
</script>

<style module>
.brutalNav {
  display: inline-block;
  padding: 0.5rem;
}

.list {
  display: flex;
  gap: 1rem; /* Más espacio para que las sombras no choquen */
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  align-items: center;
}

.brutalLink {
  padding: 0.7rem 1.4rem;
  background: white;
  /* Bordes negros gruesos obligatorios */
  border: 3px solid #000000;
  border-radius: 0; /* Neo-brutalismo suele ser cuadrado o con radio muy pequeño */
  text-decoration: none;
  color: #000000;
  font-weight: 800;
  font-size: 0.9rem;
  text-transform: uppercase; /* Da más fuerza al diseño */
  display: flex;
  align-items: center;
  gap: 0.6rem;
  
  /* Sombra sólida sin blur */
  box-shadow: 4px 4px 0px 0px #000000;
  
  transition: all 0.1s ease-in-out;
  position: relative;
}

.brutalLink:hover {
  background: var(--b-accent);
  transform: translate(-2px, -2px);
  box-shadow: 7px 7px 0px 0px #000000;
}

.brutalLink:active {
  /* Efecto de hundimiento real */
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px 0px #000000;
}

.icon {
  font-size: 1.2rem;
  /* Los iconos en este estilo a veces llevan su propio borde */
}

.label {
  letter-spacing: 0.05em;
}

/* Estilo para el link activo de Nuxt */
:global(.router-link-active) .brutalLink {
  background: var(--b-accent);
  box-shadow: 4px 4px 0px 0px #000000;
}
</style>