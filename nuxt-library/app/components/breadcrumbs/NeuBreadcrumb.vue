<template>
  <nav v-bind="$attrs" :class="$style.neuNav" :style="navStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.neuLink">
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
  color?: string; // Color base del sistema (ej: #e0e5ec)
}>();

const navStyles = computed(() => ({
  '--n-bg': props.color || '#e0e5ec',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.6)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.8)',
}));
</script>

<style module>
.neuNav {
  display: inline-block;
  padding: 1.5rem;
  background: var(--n-bg); /* El contenedor también debe ser del mismo color */
  border-radius: 2rem;
}

.list {
  display: flex;
  gap: 2rem; /* Necesita espacio para que las sombras se difuminen bien */
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  align-items: center;
}

.neuLink {
  padding: 0.8rem 1.5rem;
  background: var(--n-bg);
  text-decoration: none;
  color: #6d7c90;
  font-weight: 600;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  border-radius: 12px;
  border: none;
  
  /* ESTADO NORMAL: El botón "sale" hacia afuera */
  box-shadow: 
    6px 6px 12px var(--n-shadow-dark),
    -6px -6px 12px var(--n-shadow-light);
  
  transition: all 0.3s ease;
}

.neuLink:hover {
  color: #4b5563;
  /* Se eleva un poco más */
  box-shadow: 
    8px 8px 16px var(--n-shadow-dark),
    -8px -8px 16px var(--n-shadow-light);
}

.neuLink:active {
  /* Efecto de hundimiento físico */
  box-shadow: 
    inset 4px 4px 8px var(--n-shadow-dark),
    inset -4px -4px 8px var(--n-shadow-light);
  transform: scale(0.98);
}

/* Estado activo de Nuxt (Página actual) */
:global(.router-link-active) .neuLink {
  color: #4f46e5;
  /* El link activo se ve "presionado" permanentemente */
  box-shadow: 
    inset 3px 3px 6px var(--n-shadow-dark),
    inset -3px -3px 6px var(--n-shadow-light);
}

.icon {
  font-size: 1.1rem;
  opacity: 0.8;
}

.label {
  letter-spacing: -0.01em;
}
</style>