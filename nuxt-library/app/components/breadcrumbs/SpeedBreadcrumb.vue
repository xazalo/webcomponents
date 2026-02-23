<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.speedNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.speedLink, $style[size]]"
          :active-class="$style.activeLink"
        >
          <div :class="$style.content">
            <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
            <span :class="$style.label">{{ item.label }}</span>
          </div>
          
          <div :class="$style.shimmer" />
          <div :class="$style.trail" />
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
  accentColor?: string;   /* Color de la estela y estado activo */
  baseBg?: string;        /* Fondo del botón */
  textColor?: string;     /* Color del texto */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
  skewAngle?: number;     /* Grados de inclinación (0 a 25) */
  showShimmer?: boolean;  /* Destello al pasar el ratón */
}

const props = withDefaults(defineProps<Props>(), {
  accentColor: '#0066ff',
  baseBg: '#0a0a0a',
  textColor: '#ffffff',
  size: 'md',
  orientation: 'horizontal',
  skewAngle: 15,
  showShimmer: true
});

const navStyles = computed(() => ({
  '--s-accent': props.accentColor,
  '--s-bg': props.baseBg,
  '--s-text': props.textColor,
  '--s-skew': `-${props.skewAngle}deg`,
  '--s-skew-inv': `${props.skewAngle}deg`,
}));
</script>

<style module>
.speedNav {
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

.vertical .list { flex-direction: column; gap: 0.5rem; }

.speedLink {
  position: relative;
  background: var(--s-bg);
  color: var(--s-text);
  text-decoration: none;
  font-weight: 900;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  overflow: hidden;
  transform: skewX(var(--s-skew));
  border-left: 4px solid var(--s-accent);
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
  cursor: pointer;
}

/* Contenedor interno para enderezar el texto e icono */
.content {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  transform: skewX(var(--s-skew-inv));
  z-index: 2;
  font-style: italic;
}

/* Tamaños */
.sm { padding: 0.4rem 1.2rem; font-size: 0.7rem; border-left-width: 3px; }
.md { padding: 0.7rem 1.8rem; font-size: 0.85rem; }
.lg { padding: 1rem 2.5rem; font-size: 1.1rem; border-left-width: 6px; }

.speedLink:hover {
  transform: skewX(var(--s-skew)) translateX(8px);
  background: var(--s-bg);
  color: var(--s-accent);
  box-shadow: -10px 0 20px rgba(0, 0, 0, 0.2);
}

.activeLink {
  background: var(--s-accent) !important;
  color: #ffffff !important;
  border-left-color: #ffffff !important;
  transform: skewX(var(--s-skew)) translateX(4px);
  box-shadow: 0 0 20px rgba(var(--s-accent), 0.4);
}

/* Efecto de Estela (Trail) */
.trail {
  position: absolute;
  top: 0;
  right: 0;
  width: 0;
  height: 100%;
  background: var(--s-accent);
  opacity: 0.3;
  transition: width 0.3s ease;
  z-index: 1;
}

.speedLink:hover .trail {
  width: 100%;
}

/* Shimmer (Aceleración) */
.shimmer {
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.3),
    transparent
  );
  z-index: 3;
}

.speedLink:hover .shimmer {
  animation: speedFlash 0.6s ease-out forwards;
}

@keyframes speedFlash {
  0% { left: -100%; }
  100% { left: 150%; }
}

.icon { font-size: 1.2em; }
.label { letter-spacing: 0.05em; white-space: nowrap; }

@media (max-width: 640px) {
  .speedNav { width: 100%; overflow-x: auto; }
  .speedLink { transform: skewX(-8deg); }
  .content { transform: skewX(8deg); }
}
</style>