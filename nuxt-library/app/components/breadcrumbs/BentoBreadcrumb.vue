<template>
  <nav v-bind="$attrs" :class="$style.bentoNav" :style="navStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.bentoLink">
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
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoNav {
  display: inline-block;
  background: #f9fafb; /* Fondo sutil para el contenedor del grupo */
  padding: 0.5rem;
  border-radius: 1.5rem;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.list {
  display: flex;
  gap: 0.5rem; /* Espaciado pequeño y uniforme típico de Bento */
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  align-items: center;
}

/* En Bento no usamos flechas separadoras, cada elemento es su propio bloque */
.bentoLink {
  padding: 0.6rem 1.2rem;
  background: white;
  border: 1px solid rgba(0, 0, 0, 0.03);
  border-radius: 1rem; /* Radio grande y uniforme */
  text-decoration: none;
  color: #1f2937;
  font-weight: 600;
  font-size: 0.85rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  box-shadow: 
    0 1px 3px rgba(0,0,0,0.02),
    0 4px 12px rgba(0,0,0,0.03);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.bentoLink:hover {
  background: white;
  transform: scale(1.03);
  box-shadow: 
    0 10px 15px -3px rgba(0, 0, 0, 0.08),
    0 4px 6px -4px rgba(0, 0, 0, 0.04);
  color: var(--b-accent);
}

.icon {
  font-size: 1.1rem;
}

.label {
  letter-spacing: -0.01em;
}

/* Estilo para el link activo (opcional) */
:global(.router-link-active) .bentoLink {
  background: #f3f4f6;
  border-color: rgba(0, 0, 0, 0.1);
}
</style>