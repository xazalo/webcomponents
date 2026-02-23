<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.glassNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.link, $style[size]]"
          :active-class="$style.activeLink"
        >
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
        </NuxtLink>
        
        <span v-if="index < items.length - 1" :class="$style.separator">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
        </span>
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
  // Parámetros de Cristal
  blur?: string;          /* Intensidad del desenfoque (ej: 12px) */
  opacity?: number;       /* Transparencia del fondo (0 a 1) */
  tintColor?: string;     /* Color del tinte (blanco, azulado, etc) */
  accentColor?: string;   /* Color del estado activo */
  // Estructura
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  blur: '10px',
  opacity: 0.3,
  tintColor: '255, 255, 255', // Formato RGB para manejar alfa
  accentColor: '#10b981',     // Verde esmeralda para un toque premium
  size: 'md',
  orientation: 'horizontal',
  borderRadius: '50px'
});

const navStyles = computed(() => ({
  '--g-blur': props.blur,
  '--g-bg': `rgba(${props.tintColor}, ${props.opacity})`,
  '--g-border': `rgba(${props.tintColor}, 0.3)`,
  '--g-accent': props.accentColor,
  '--g-radius': props.borderRadius,
}));
</script>

<style module>
.glassNav {
  display: inline-block;
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur));
  -webkit-backdrop-filter: blur(var(--g-blur));
  padding: 0.4rem;
  border-radius: var(--g-radius);
  border: 1px solid var(--g-border);
  /* Sombra sutil pero profunda para elevar el cristal */
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; align-items: stretch; }

.item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.link {
  text-decoration: none;
  color: #1f2937;
  font-weight: 600;
  border-radius: calc(var(--g-radius) * 0.8);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  opacity: 0.6;
}

/* Tamaños */
.sm { padding: 0.3rem 0.8rem; font-size: 0.75rem; }
.md { padding: 0.5rem 1.1rem; font-size: 0.85rem; }
.lg { padding: 0.7rem 1.5rem; font-size: 1rem; }

.link:hover {
  opacity: 1;
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-1px);
}

.activeLink {
  opacity: 1;
  background: rgba(255, 255, 255, 0.4);
  color: var(--g-accent) !important;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.separator {
  display: flex;
  align-items: center;
  color: #1f2937;
  opacity: 0.2;
}

.vertical .separator {
  transform: rotate(90deg);
  justify-content: center;
  padding: 0.2rem 0;
}

.icon { font-size: 1.2em; opacity: 0.8; }
.label { letter-spacing: -0.01em; }

@media (max-width: 640px) {
  .glassNav { width: 100%; overflow-x: auto; }
}
</style>