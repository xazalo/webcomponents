<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.neuRoot, { [$style.isActive]: modelValue }]"
    :style="neuStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.neuIndicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
      <span :class="$style.statusText">
        {{ modelValue ? onText : offText }}
      </span>
    </div>
  </button>
</template>

<script setup lang="ts">
import { computed } from 'vue';

defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label: string;
  onText?: string;
  offText?: string;
  // Neu Tuning
  baseColor?: string;     /* Color de fondo y piezas (Eje: #e0e5ec) */
  accentColor?: string;   /* Color del estado activo */
  intensity?: number;     /* Fuerza de la sombra (0.1 a 1) */
  distance?: number;      /* Distancia de la sombra (px) */
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'ON',
  offText: 'OFF',
  baseColor: '#e0e5ec',
  accentColor: '#6d5dfc',
  intensity: 0.2,
  distance: 6,
  borderRadius: '20px'
});

defineEmits(['update:modelValue']);

const neuStyles = computed(() => {
  // Lógica para calcular sombras claras y oscuras basadas en el color base
  // Nota: En una implementación real, podrías usar una función JS para oscurecer/aclarar
  // Aquí usamos valores dinámicos para el CSS
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.accentColor,
    '--n-radius': props.borderRadius,
    '--n-dist': `${props.distance}px`,
    '--n-dist-neg': `-${props.distance}px`,
    '--n-blur': `${props.distance * 2}px`,
    '--n-intensity': props.intensity,
  };
});
</script>

<style module>
.neuRoot {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: var(--n-bg);
  border-radius: var(--n-radius);
  border: none;
  cursor: pointer;
  /* El volumen exterior del contenedor */
  box-shadow: 
    var(--n-dist) var(--n-dist) var(--n-blur) rgba(163, 177, 198, 0.6), 
    var(--n-dist-neg) var(--n-dist-neg) var(--n-blur) rgba(255, 255, 255, 0.8);
  transition: all 0.3s ease;
  width: fit-content;
  outline: none;
}

.neuRoot:active {
  /* Al presionar, el contenedor parece hundirse */
  box-shadow: 
    inset 2px 2px 5px rgba(163, 177, 198, 0.6), 
    inset -2px -2px 5px rgba(255, 255, 255, 0.8);
  transform: scale(0.98);
}

.cell {
  background: var(--n-bg);
  padding: 10px 16px;
  border-radius: calc(var(--n-radius) - 6px);
  display: flex;
  align-items: center;
  gap: 12px;
  /* Elevación individual de celdas */
  box-shadow: 
    4px 4px 8px rgba(163, 177, 198, 0.5), 
    -4px -4px 8px rgba(255, 255, 255, 0.8);
}

.label {
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  color: #7a8ba9;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.neuIndicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  transition: all 0.3s ease;
  /* Efecto de hueco pequeño */
  box-shadow: 
    inset 1px 1px 2px rgba(163, 177, 198, 0.8), 
    inset -1px -1px 2px rgba(255, 255, 255, 0.8);
}

.bgActive { 
  background: var(--n-accent); 
  box-shadow: 0 0 10px color-mix(in srgb, var(--n-accent), transparent 40%);
}

.bgOff { background: #d1d9e6; }

.toggleCell {
  min-width: 100px;
  justify-content: space-between;
}

.track {
  width: 42px;
  height: 22px;
  background: var(--n-bg);
  border-radius: 11px;
  position: relative;
  /* El carril siempre está hundido */
  box-shadow: 
    inset 3px 3px 6px rgba(163, 177, 198, 0.6), 
    inset -3px -3px 6px rgba(255, 255, 255, 0.8);
}

.thumb {
  width: 16px;
  height: 16px;
  background: var(--n-bg);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  /* El botón deslizante sobresale del carril */
  box-shadow: 
    2px 2px 4px rgba(163, 177, 198, 0.5), 
    -1px -1px 2px rgba(255, 255, 255, 0.8);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 800;
  color: #9baacf;
  transition: color 0.3s ease;
}

/* --- ESTADOS ACTIVOS --- */

.isActive .thumb {
  transform: translateX(20px);
  background: var(--n-accent);
  box-shadow: 0 0 12px color-mix(in srgb, var(--n-accent), transparent 50%);
}

.isActive .statusText {
  color: var(--n-accent);
}
</style>