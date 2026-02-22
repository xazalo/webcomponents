<template>
  <nav v-bind="$attrs" :class="$style.clayNav" :style="navStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.clayLink">
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
  color?: string;
}>();

const navStyles = computed(() => ({
  '--c-bg': props.color || '#f0f3ff', // Colores pastel funcionan mejor
  '--c-accent': '#6366f1',
}));
</script>

<style module>
.clayNav {
  display: inline-block;
  padding: 1rem;
}

.list {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  align-items: center;
}

.clayLink {
  padding: 0.8rem 1.6rem;
  background: var(--c-bg);
  text-decoration: none;
  color: #475569;
  font-weight: 700;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  border-radius: 20px; /* Bordes muy redondeados */
  
  /* CLAY EFFECT: Sombra exterior suave + Sombras internas (brillo y profundidad) */
  box-shadow: 
    0 10px 20px -5px rgba(0, 0, 0, 0.1),
    inset 0 4px 6px rgba(255, 255, 255, 0.8),
    inset 0 -4px 8px rgba(0, 0, 0, 0.05);
  
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.clayLink:hover {
  transform: translateY(-4px) scale(1.02);
  color: var(--c-accent);
  box-shadow: 
    0 15px 25px -5px rgba(0, 0, 0, 0.12),
    inset 0 4px 6px rgba(255, 255, 255, 1),
    inset 0 -4px 10px rgba(0, 0, 0, 0.08);
}

.clayLink:active {
  transform: translateY(2px) scale(0.98);
  box-shadow: 
    0 5px 10px -3px rgba(0, 0, 0, 0.1),
    inset 0 2px 4px rgba(255, 255, 255, 0.5),
    inset 0 -2px 6px rgba(0, 0, 0, 0.1);
}

.icon {
  font-size: 1.2rem;
  filter: drop-shadow(0 2px 2px rgba(0,0,0,0.05));
}

.label {
  letter-spacing: -0.01em;
}

/* Estilo para el link activo de Nuxt */
:global(.router-link-active) .clayLink {
  background: white;
  color: var(--c-accent);
  box-shadow: 
    0 10px 20px -5px rgba(99, 102, 241, 0.2),
    inset 0 4px 6px rgba(255, 255, 255, 0.8),
    inset 0 -4px 8px rgba(99, 102, 241, 0.1);
}
</style>