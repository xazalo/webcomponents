<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.neuNav, $style[orientation]]" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.neuLink, $style[size]]"
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
  // Colores (Marca Blanca)
  baseColor?: string;     /* El color de la superficie (ej: #e0e5ec) */
  accentColor?: string;   /* Color del texto/icono cuando está activo */
  // Parámetros Físicos
  depth?: number;         /* Qué tanto sobresale (1 a 20) */
  intensity?: number;     /* Opacidad de las sombras (0.1 a 1) */
  borderRadius?: string;  /* Curvatura de los botones */
  // Estructura
  size?: 'sm' | 'md' | 'lg';
  orientation?: 'horizontal' | 'vertical';
}

const props = withDefaults(defineProps<Props>(), {
  baseColor: '#e0e5ec',
  accentColor: '#4f46e5',
  depth: 6,
  intensity: 0.5,
  borderRadius: '12px',
  size: 'md',
  orientation: 'horizontal'
});

const navStyles = computed(() => {
  // Helpers simples para simular sombras basadas en el color base
  // En un entorno pro, usarías una librería como 'polished' o 'chromajs'
  // Aquí usamos filtros CSS para que sea agnóstico
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.accentColor,
    '--n-radius': props.borderRadius,
    '--n-depth': `${props.depth}px`,
    '--n-depth-neg': `-${props.depth}px`,
    '--n-depth-inner': `${Math.floor(props.depth * 0.6)}px`,
    '--n-shadow-dark': `rgba(163, 177, 198, ${props.intensity})`,
    '--n-shadow-light': `rgba(255, 255, 255, ${props.intensity + 0.2})`,
  };
});
</script>

<style module>
.neuNav {
  display: inline-block;
  background: var(--n-bg);
  padding: 1rem;
  border-radius: calc(var(--n-radius) * 2);
}

.vertical { display: block; width: max-content; }

.list {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.vertical .list { flex-direction: column; }

.neuLink {
  position: relative;
  text-decoration: none;
  background: var(--n-bg);
  color: #6d7c90;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  border-radius: var(--n-radius);
  border: none;
  
  /* EXTRUSIÓN INICIAL */
  box-shadow: 
    var(--n-depth) var(--n-depth) calc(var(--n-depth) * 2) var(--n-shadow-dark),
    var(--n-depth-neg) var(--n-depth-neg) calc(var(--n-depth) * 2) var(--n-shadow-light);
  
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
}

/* Tamaños */
.sm { padding: 0.5rem 1rem; font-size: 0.75rem; }
.md { padding: 0.8rem 1.5rem; font-size: 0.9rem; }
.lg { padding: 1.1rem 2.2rem; font-size: 1.1rem; }

.neuLink:hover {
  color: #4b5563;
  /* El botón se eleva más */
  transform: translateY(-2px);
  box-shadow: 
    calc(var(--n-depth) * 1.5) calc(var(--n-depth) * 1.5) calc(var(--n-depth) * 3) var(--n-shadow-dark),
    calc(var(--n-depth-neg) * 1.5) calc(var(--n-depth-neg) * 1.5) calc(var(--n-depth) * 3) var(--n-shadow-light);
}

.activeLink {
  color: var(--n-accent) !important;
  /* HUNDIMIENTO (INSET) */
  box-shadow: 
    inset var(--n-depth-inner) var(--n-depth-inner) calc(var(--n-depth-inner) * 2) var(--n-shadow-dark),
    inset calc(var(--n-depth-inner) * -1) calc(var(--n-depth-inner) * -1) calc(var(--n-depth-inner) * 2) var(--n-shadow-light);
  transform: scale(0.98);
}

.neuLink:active {
  transform: scale(0.95);
}

.icon { font-size: 1.2em; opacity: 0.7; }
.label { letter-spacing: -0.01em; }

@media (max-width: 640px) {
  .neuNav { width: 100%; overflow-x: auto; }
}
</style>