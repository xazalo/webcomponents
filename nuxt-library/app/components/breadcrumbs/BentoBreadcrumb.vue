<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.bentoNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.bentoLink, $style[size]]"
          :active-class="$style.activeLink"
        >
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
          <div :class="$style.activeIndicator" />
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
  // Colores principales
  accentColor?: string;     /* Color de texto activo e indicador */
  navBg?: string;           /* Fondo del contenedor padre */
  itemBg?: string;          /* Fondo de cada botón/tile */
  textColor?: string;       /* Color de texto base */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  borderRadius?: string;
  shadowIntensity?: number; /* De 0 a 1 */
}

const props = withDefaults(defineProps<Props>(), {
  accentColor: '#4f46e5',
  navBg: '#f1f3f5',
  itemBg: '#ffffff',
  textColor: '#374151',
  size: 'md',
  orientation: 'horizontal',
  borderRadius: '1rem',
  shadowIntensity: 0.1
});

const navStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-nav-bg': props.navBg,
  '--b-item-bg': props.itemBg,
  '--b-text': props.textColor,
  '--b-radius': props.borderRadius,
  '--b-radius-outer': `calc(${props.borderRadius} + 0.5rem)`,
  '--b-shadow-alpha': props.shadowIntensity,
}));
</script>

<style module>
.bentoNav {
  display: inline-block;
  background: var(--b-nav-bg);
  padding: 0.5rem;
  border-radius: var(--b-radius-outer);
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  gap: 0.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; }

.bentoLink {
  position: relative;
  background: var(--b-item-bg);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: var(--b-radius);
  text-decoration: none;
  color: var(--b-text);
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  /* Sombra dinámica basada en intensidad */
  box-shadow: 0 4px 12px rgba(0, 0, 0, var(--b-shadow-alpha));
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
}

.sm { padding: 0.4rem 0.8rem; font-size: 0.75rem; }
.md { padding: 0.6rem 1.2rem; font-size: 0.85rem; }
.lg { padding: 0.8rem 1.6rem; font-size: 1rem; }

.bentoLink:hover {
  transform: translateY(-2px);
  color: var(--b-accent);
  filter: brightness(1.02);
  box-shadow: 0 8px 16px rgba(0, 0, 0, calc(var(--b-shadow-alpha) + 0.05));
}

.activeLink {
  background: var(--b-item-bg);
  color: var(--b-accent);
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.05);
}

.activeIndicator {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 3px;
  background: var(--b-accent);
  border-radius: 3px 3px 0 0;
  transition: all 0.3s ease;
  transform: translateX(-50%);
}

.activeLink .activeIndicator { width: 35%; }

.icon { font-size: 1.2em; line-height: 1; }
.label { letter-spacing: -0.01em; white-space: nowrap; }

@media (max-width: 640px) {
  .bentoNav {
    width: 100%;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
  }
}
</style>