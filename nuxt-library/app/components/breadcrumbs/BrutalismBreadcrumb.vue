<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.brutalNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.brutalLink, $style[size]]"
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
  accentColor?: string;   /* Color al pasar el ratón / activo */
  baseColor?: string;     /* Color de fondo del botón */
  shadowColor?: string;   /* Color de la sombra sólida */
  borderColor?: string;   /* Color del borde */
  textColor?: string;     /* Color del texto */
  // Estructura
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  borderWidth?: string;   /* Grosor del trazo */
  shadowSize?: string;    /* Qué tanto sobresale la sombra */
  borderRadius?: string;  /* Por defecto 0 (cuadrado) */
}

const props = withDefaults(defineProps<Props>(), {
  accentColor: '#bc95ff',
  baseColor: '#ffffff',
  shadowColor: '#000000',
  borderColor: '#000000',
  textColor: '#000000',
  size: 'md',
  orientation: 'horizontal',
  borderWidth: '3px',
  shadowSize: '5px',
  borderRadius: '0px'
});

const navStyles = computed(() => ({
  '--brutal-accent': props.accentColor,
  '--brutal-bg': props.baseColor,
  '--brutal-shadow': props.shadowColor,
  '--brutal-border': props.borderColor,
  '--brutal-text': props.textColor,
  '--brutal-bw': props.borderWidth,
  '--brutal-sw': props.shadowSize,
  '--brutal-radius': props.borderRadius,
}));
</script>

<style module>
.brutalNav {
  display: inline-block;
  padding: 0.5rem;
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  gap: calc(var(--brutal-sw) + 0.5rem);
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; }

.brutalLink {
  position: relative;
  padding: 0.7rem 1.4rem;
  background: var(--brutal-bg);
  border: var(--brutal-bw) solid var(--brutal-border);
  border-radius: var(--brutal-radius);
  text-decoration: none;
  color: var(--brutal-text);
  font-weight: 900;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  /* Sombra brutalista pura */
  box-shadow: var(--brutal-sw) var(--brutal-sw) 0px 0px var(--brutal-shadow);
  transition: all 0.1s cubic-bezier(0.17, 0.67, 0.83, 0.67);
  white-space: nowrap;
}

/* Tamaños */
.sm { padding: 0.4rem 0.8rem; font-size: 0.7rem; --brutal-sw: 3px; }
.md { padding: 0.7rem 1.4rem; font-size: 0.9rem; }
.lg { padding: 1rem 2rem; font-size: 1.1rem; --brutal-sw: 8px; }

.brutalLink:hover {
  background: var(--brutal-accent);
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--brutal-sw) + 2px) calc(var(--brutal-sw) + 2px) 0px 0px var(--brutal-shadow);
}

.activeLink {
  background: var(--brutal-accent);
}

.brutalLink:active {
  transform: translate(var(--brutal-sw), var(--brutal-sw));
  box-shadow: 0px 0px 0px 0px var(--brutal-shadow);
}

.icon { font-size: 1.2em; }
.label { letter-spacing: 0.05em; }

@media (max-width: 640px) {
  .brutalNav {
    width: 100%;
    overflow-x: auto;
    padding-bottom: 1rem;
  }
}
</style>