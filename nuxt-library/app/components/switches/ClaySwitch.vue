<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.clayRoot, { [$style.isActive]: modelValue }]"
    :style="clayStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.clayIndicator, modelValue ? $style.bgOn : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.controlCell]">
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
  // Clay Tuning
  baseColor?: string;     /* Color de la "arcilla" (Fondo) */
  accentColor?: string;   /* Color del track cuando está ON */
  successColor?: string;  /* Color del mini led cuando está ON */
  borderRadius?: string;
  depth?: number;         /* Intensidad de la sombra (1 a 10) */
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'ON',
  offText: 'OFF',
  baseColor: '#f0f3f9',
  accentColor: '#6366f1',
  successColor: '#22c55e',
  borderRadius: '16px',
  depth: 8
});

defineEmits(['update:modelValue']);

const clayStyles = computed(() => ({
  '--c-bg': props.baseColor,
  '--c-accent': props.accentColor,
  '--c-success': props.successColor,
  '--c-radius': props.borderRadius,
  '--c-d-out': `${props.depth}px`,
  '--c-d-in': `${props.depth / 2}px`,
  // Generación de sombras basadas en la profundidad
  '--c-sh-dark': 'rgba(0, 0, 0, 0.08)',
  '--c-sh-light': 'rgba(255, 255, 255, 0.8)'
}));
</script>

<style module>
.clayRoot {
  display: flex;
  gap: 12px;
  padding: 12px;
  background: var(--c-bg);
  border-radius: calc(var(--c-radius) * 1.5);
  border: none;
  cursor: pointer;
  box-shadow: 
    var(--c-d-out) var(--c-d-out) calc(var(--c-d-out) * 2) var(--c-sh-dark), 
    calc(var(--c-d-out) * -1) calc(var(--c-d-out) * -1) calc(var(--c-d-out) * 2) var(--c-sh-light);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  outline: none;
  width: fit-content;
}

.clayRoot:active {
  transform: scale(0.96);
  box-shadow: 
    calc(var(--c-d-in)) calc(var(--c-d-in)) var(--c-d-out) var(--c-sh-dark);
}

.cell {
  background: var(--c-bg);
  padding: 10px 16px;
  border-radius: var(--c-radius);
  display: flex;
  align-items: center;
  gap: 12px;
  /* El efecto Clay: Sombra exterior Y sombra interior */
  box-shadow: 
    var(--c-d-in) var(--c-d-in) var(--c-d-out) var(--c-sh-dark), 
    calc(var(--c-d-in) * -1) calc(var(--c-d-in) * -1) var(--c-d-out) var(--c-sh-light),
    inset 2px 2px 4px rgba(0, 0, 0, 0.04), 
    inset -2px -2px 4px rgba(255, 255, 255, 0.7);
}

.label {
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 0.75rem;
  font-weight: 800;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  transition: color 0.3s ease;
}

.clayIndicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.bgOn { 
  background: var(--c-success); 
  box-shadow: 
    inset 2px 2px 4px rgba(0,0,0,0.2), 
    0 0 10px color-mix(in srgb, var(--c-success), transparent 40%);
}

.bgOff { 
  background: #cbd5e1; 
  box-shadow: inset 1px 1px 2px rgba(0,0,0,0.1);
}

.controlCell {
  min-width: 100px;
  justify-content: space-between;
}

.track {
  width: 40px;
  height: 22px;
  background: #e2e8f0;
  border-radius: 12px;
  position: relative;
  transition: all 0.3s ease;
  box-shadow: 
    inset 3px 3px 6px rgba(0, 0, 0, 0.1), 
    inset -3px -3px 6px rgba(255, 255, 255, 0.5);
}

.thumb {
  width: 16px;
  height: 16px;
  background: var(--c-bg);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  /* Sombra del botón que lo hace parecer flotar sobre el track */
  box-shadow: 
    2px 2px 4px rgba(0, 0, 0, 0.1), 
    inset 1px 1px 2px rgba(255, 255, 255, 1);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 900;
  color: #94a3b8;
  font-variant-numeric: tabular-nums;
}

/* --- ESTADOS ACTIVOS --- */

.isActive .label { color: var(--c-accent); }

.isActive .track {
  background: var(--c-accent);
  box-shadow: 
    inset 3px 3px 8px rgba(0, 0, 0, 0.3),
    0 0 15px color-mix(in srgb, var(--c-accent), transparent 60%);
}

.isActive .thumb {
  transform: translateX(18px);
  background: #ffffff;
}

.isActive .statusText { color: var(--c-accent); }
</style>