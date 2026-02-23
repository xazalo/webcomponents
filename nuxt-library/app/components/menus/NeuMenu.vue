<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.neuMenu, $style[size]]" 
    :style="neuStyles"
  >
    <div :class="[$style.cell, $style.headerCell, $style.flat]">
      <span :class="$style.label">{{ prefix }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
    </div>

    <div :class="$style.list">
      <a 
        v-for="link in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          { [$style.active]: modelValue === link.path }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="[$style.statusIcon, { [$style.activeIcon]: modelValue === link.path }]" />
      </a>
    </div>

    <button 
      v-if="buttonText"
      :class="[$style.cell, $style.actionBtn]"
      @click="$emit('action')"
    >
      {{ buttonText }}
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

interface Props {
  modelValue: string;
  links: NavLink[];
  buttonText?: string;
  prefix?: string;
  // Personalización Neumórfica
  baseColor?: string;     /* El color del "plástico" (ej: #e0e5ec) */
  accentColor?: string;   /* Color para estados activos */
  intensity?: number;     /* Fuerza de la sombra (0.1 a 1) */
  distance?: number;      /* Qué tanto sobresale (en px) */
  borderRadius?: string;
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '',
  prefix: 'CONTROL_PANEL',
  baseColor: '#e0e5ec',
  accentColor: '#6d5dfc',
  intensity: 0.2,
  distance: 6,
  borderRadius: '20px',
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});

/**
 * Lógica para generar sombras más oscuras/claras basadas en el baseColor
 * Para un control total, usamos filtros de opacidad sobre negro y blanco
 */
const neuStyles = computed(() => {
  const d = props.distance;
  const i = props.intensity;
  
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.accentColor,
    '--n-radius': props.borderRadius,
    // Sombras exteriores (Extrusión)
    '--n-shadow-out': `${d}px ${d}px ${d*2}px rgba(163, 177, 198, ${i * 3}), -${d}px -${d}px ${d*2}px rgba(255, 255, 255, 1)`,
    // Sombras interiores (Concavidad)
    '--n-shadow-in': `inset ${d/1.5}px ${d/1.5}px ${d}px rgba(163, 177, 198, ${i * 3.5}), inset -${d/1.5}px -${d/1.5}px ${d}px rgba(255, 255, 255, 1)`,
    '--n-glow': `${props.accentColor}66`
  };
});
</script>

<style module>
.neuMenu {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 30px;
  background: var(--n-bg);
  border-radius: calc(var(--n-radius) * 2);
  width: 100%;
  max-width: 380px;
  box-shadow: var(--n-shadow-out);
  font-family: 'Segoe UI', system-ui, sans-serif;
}

.cell {
  background: var(--n-bg);
  border-radius: var(--n-radius);
  padding: 18px 22px;
  border: none;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  position: relative;
}

/* --- Estados de Relieve --- */
.flat {
  box-shadow: var(--n-shadow-out);
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
  box-shadow: var(--n-shadow-out);
}

.linkCell:hover {
  transform: translateY(-2px);
  filter: brightness(1.02);
}

.active {
  box-shadow: var(--n-shadow-in) !important;
  transform: translateY(0) scale(0.98);
}

/* --- Contenido --- */
.label {
  font-size: 0.65rem;
  font-weight: 800;
  color: #7a8ba9;
  letter-spacing: 1.5px;
}

.title {
  margin: 5px 0 0;
  font-size: 1.4rem;
  color: #44475a;
  font-weight: 700;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.linkTitle {
  display: block;
  font-weight: 700;
  font-size: 1.05rem;
  color: #44475a;
  transition: color 0.3s ease;
}

.active .linkTitle {
  color: var(--n-accent);
}

.linkDesc {
  font-size: 0.75rem;
  color: #7a8ba9;
}

/* --- LED Status --- */
.statusIcon {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--n-bg);
  box-shadow: var(--n-shadow-out);
  transition: all 0.3s ease;
}

.activeIcon {
  background: var(--n-accent);
  box-shadow: 0 0 10px var(--n-glow);
}

/* --- Acción --- */
.actionBtn {
  margin-top: 15px;
  font-weight: 800;
  color: #ff7675;
  cursor: pointer;
  box-shadow: var(--n-shadow-out);
  text-transform: uppercase;
  letter-spacing: 1px;
  font-size: 0.8rem;
}

.actionBtn:active {
  box-shadow: var(--n-shadow-in);
  transform: scale(0.97);
}

/* --- Tamaños --- */
.sm { padding: 20px; max-width: 320px; }
.sm .title { font-size: 1.1rem; }
.lg { padding: 40px; max-width: 440px; }
.lg .title { font-size: 1.8rem; }
</style>