<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.clayNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.clayLink, $style[size]]"
          :active-class="$style.activeLink"
        >
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

interface Props {
  items: NavItem[];
  // Colores
  baseColor?: string;     /* El color principal del "barro" */
  accentColor?: string;   /* Color del texto e icono activo */
  textColor?: string;     /* Color del texto base */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  borderRadius?: string;
  depth?: number;         /* Intensidad del relieve (0 a 1) */
}

const props = withDefaults(defineProps<Props>(), {
  baseColor: '#f0f3ff',
  accentColor: '#6366f1',
  textColor: '#475569',
  size: 'md',
  orientation: 'horizontal',
  borderRadius: '20px',
  depth: 0.4
});

const navStyles = computed(() => ({
  '--clay-bg': props.baseColor,
  '--clay-accent': props.accentColor,
  '--clay-text': props.textColor,
  '--clay-radius': props.borderRadius,
  // Cálculo de sombras internas dinámicas basadas en la profundidad
  '--clay-shadow-inner': `inset 0 -${8 * props.depth}px ${12 * props.depth}px rgba(0,0,0,${0.1 * props.depth})`,
  '--clay-highlight': `inset 0 ${6 * props.depth}px ${8 * props.depth}px rgba(255,255,255,0.8)`,
  '--clay-shadow-outer': `0 ${15 * props.depth}px ${25 * props.depth}px -5px rgba(0,0,0,${0.1 * props.depth})`,
}));
</script>

<style module>
.clayNav {
  display: inline-block;
  padding: 0.5rem;
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  gap: 1rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; }

.clayLink {
  position: relative;
  background: var(--clay-bg);
  color: var(--clay-text);
  text-decoration: none;
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  border-radius: var(--clay-radius);
  
  /* Efecto Claymorphic Multicapa */
  box-shadow: 
    var(--clay-shadow-outer),
    var(--clay-highlight),
    var(--clay-shadow-inner);
  
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border: none;
  cursor: pointer;
}

/* Tamaños */
.sm { padding: 0.5rem 1rem; font-size: 0.75rem; }
.md { padding: 0.8rem 1.6rem; font-size: 0.9rem; }
.lg { padding: 1.1rem 2.2rem; font-size: 1.1rem; }

.clayLink:hover {
  transform: translateY(-4px) scale(1.02);
  color: var(--clay-accent);
  filter: brightness(1.03);
}

.clayLink:active {
  transform: translateY(2px) scale(0.97);
  box-shadow: 
    0 4px 8px rgba(0,0,0,0.1),
    inset 0 2px 4px rgba(255,255,255,0.5),
    inset 0 -2px 6px rgba(0,0,0,0.05);
}

.activeLink {
  background: #ffffff;
  color: var(--clay-accent) !important;
  /* Sombra de "glow" suave para el estado activo */
  box-shadow: 
    0 10px 20px -5px rgba(99, 102, 241, 0.25),
    var(--clay-highlight),
    inset 0 -4px 8px rgba(99, 102, 241, 0.05);
}

.icon {
  font-size: 1.2em;
  opacity: 0.9;
}

.label {
  letter-spacing: -0.01em;
}

@media (max-width: 640px) {
  .clayNav {
    width: 100%;
    overflow-x: auto;
    padding-bottom: 1rem;
  }
}
</style>