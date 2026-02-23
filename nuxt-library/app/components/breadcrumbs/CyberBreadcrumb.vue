<template>
  <nav 
    v-bind="$attrs" 
    :class="$style.breadcrumbRoot" 
    :style="navStyles"
  >
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[
            $style.link, 
            $style[size],
            { [$style.active]: index === items.length - 1 }
          ]"
        >
          <span v-if="item.icon" :class="$style.icon">{{ item.icon }}</span>
          <span :class="$style.label">{{ item.label }}</span>
          
          <div v-if="index < items.length - 1" :class="$style.arrowHead" />
        </NuxtLink>
      </li>
    </ol>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface BreadcrumbItem {
  label: string;
  to: string;
  icon?: string;
}

interface Props {
  items: BreadcrumbItem[];
  accentColor?: string;   /* Color del texto activo y hover */
  baseBg?: string;        /* Fondo de los eslabones */
  textColor?: string;     /* Color de texto inactivo */
  size?: 'sm' | 'md' | 'lg';
  angle?: number;         /* Inclinación del corte (0 a 20) */
}

const props = withDefaults(defineProps<Props>(), {
  accentColor: '#4f46e5',
  baseBg: '#ffffff',
  textColor: '#6b7280',
  size: 'md',
  angle: 15
});

const navStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-bg': props.baseBg,
  '--b-text': props.textColor,
  '--b-angle': `${props.angle}%`,
  '--b-angle-inv': `${100 - props.angle}%`,
}));
</script>

<style module>
.breadcrumbRoot {
  display: inline-block;
  filter: drop-shadow(0 4px 6px rgba(0,0,0,0.05));
}

.list {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  /* Margen negativo para que los elementos se encajen entre sí */
  gap: 0;
}

.item {
  display: flex;
  align-items: center;
}

.link {
  position: relative;
  text-decoration: none;
  background: var(--b-bg);
  color: var(--b-text);
  font-weight: 700;
  display: flex;
  align-items: center;
  transition: all 0.3s ease;
  
  /* Corte geométrico tipo flecha/fletcha */
  clip-path: polygon(
    0% 0%, 
    var(--b-angle-inv) 0%, 
    100% 50%, 
    var(--b-angle-inv) 100%, 
    0% 100%, 
    var(--b-angle) 50%
  );
  
  /* Compensación de padding para el corte izquierdo */
  padding-left: 2rem;
  padding-right: 2.5rem;
}

/* Primer elemento: No tiene corte de entrada (flecha) */
.item:first-child .link {
  clip-path: polygon(
    0% 0%, 
    var(--b-angle-inv) 0%, 
    100% 50%, 
    var(--b-angle-inv) 100%, 
    0% 100%
  );
  padding-left: 1.2rem;
  border-radius: 8px 0 0 8px;
}

/* Último elemento: Es el estado activo */
.active {
  background: var(--b-accent);
  color: #ffffff !important;
  pointer-events: none;
  padding-right: 1.5rem;
  /* El último no necesita punta de flecha de salida */
  clip-path: polygon(
    0% 0%, 
    100% 0%, 
    100% 100%, 
    0% 100%, 
    var(--b-angle) 50%
  );
  border-radius: 0 8px 8px 0;
}

/* Tamaños */
.sm { height: 32px; font-size: 0.75rem; padding-left: 1.5rem; padding-right: 2rem; }
.md { height: 44px; font-size: 0.85rem; }
.lg { height: 56px; font-size: 1rem; padding-left: 2.5rem; padding-right: 3.5rem; }

.link:hover:not(.active) {
  background: #f9fafb;
  color: var(--b-accent);
  transform: translateX(4px);
  z-index: 10;
}

.icon {
  margin-right: 0.5rem;
  font-size: 1.2em;
}

.label {
  white-space: nowrap;
  letter-spacing: -0.01em;
}

@media (max-width: 640px) {
  .label { display: none; }
  .icon { margin-right: 0; }
  .link { padding-left: 1.5rem; padding-right: 2rem; }
}
</style>