<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.bubbleNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.bubbleLink, $style[size]]"
          :active-class="$style.activeLink"
        >
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
        </NuxtLink>
        
        <span v-if="index < items.length - 1 && showSeparator" :class="$style.separator">
          {{ separator }}
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
  // Colores
  accentColor?: string;   /* Color al pasar el ratón / fondo activo */
  itemBg?: string;        /* Fondo base de la burbuja */
  textColor?: string;     /* Color del texto */
  borderColor?: string;   /* Color del borde (opcional) */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  showSeparator?: boolean;
  separator?: string;
  shadowIntensity?: number;
}

const props = withDefaults(defineProps<Props>(), {
  accentColor: '#f43f5e',   /* Rosa vibrante tipo chicle */
  itemBg: '#ffffff',
  textColor: '#4b5563',
  borderColor: 'transparent',
  size: 'md',
  orientation: 'horizontal',
  showSeparator: false,
  separator: '•',
  shadowIntensity: 0.08
});

const navStyles = computed(() => ({
  '--bubble-accent': props.accentColor,
  '--bubble-bg': props.itemBg,
  '--bubble-text': props.textColor,
  '--bubble-border': props.borderColor,
  '--bubble-shadow': `0 8px 20px rgba(0, 0, 0, ${props.shadowIntensity})`,
}));
</script>

<style module>
.bubbleNav {
  display: inline-block;
  padding: 0.5rem;
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; align-items: flex-start; }

.item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.bubbleLink {
  text-decoration: none;
  background: var(--bubble-bg);
  color: var(--bubble-text);
  font-weight: 700;
  border: 2px solid var(--bubble-border);
  /* El secreto: radios de esquina asimétricos y fluidos */
  border-radius: 25px 15px 30px 12px;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  box-shadow: var(--bubble-shadow);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  white-space: nowrap;
}

/* Tamaños */
.sm { padding: 0.3rem 0.9rem; font-size: 0.75rem; }
.md { padding: 0.5rem 1.2rem; font-size: 0.85rem; }
.lg { padding: 0.8rem 1.8rem; font-size: 1.1rem; }

.bubbleLink:hover {
  transform: scale(1.05) rotate(-1deg);
  border-color: var(--bubble-accent);
  color: var(--bubble-accent);
  /* Cambia la forma ligeramente al pasar el ratón */
  border-radius: 15px 25px 12px 30px;
}

.activeLink {
  background: var(--bubble-accent);
  color: #ffffff !important;
  border-color: var(--bubble-accent);
  transform: scale(1.02);
}

.separator {
  color: #d1d5db;
  font-weight: bold;
  opacity: 0.5;
}

.icon { font-size: 1.2em; }

.label {
  letter-spacing: -0.01em;
}

@media (max-width: 640px) {
  .bubbleNav {
    width: 100%;
    overflow-x: auto;
    padding-bottom: 0.5rem;
  }
}
</style>