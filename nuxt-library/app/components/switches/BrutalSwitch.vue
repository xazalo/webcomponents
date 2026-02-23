<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.brutalRoot, { [$style.isActive]: modelValue }]"
    :style="brutalStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.indicator, modelValue ? $style.bgOn : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <span :class="$style.statusText">
        {{ modelValue ? onText : offText }}
      </span>
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
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
  // Brutal Tuning
  bgIdle?: string;      /* Color de fondo desactivado (ej: Amarillo) */
  bgActive?: string;    /* Color de fondo activado (ej: Cyan) */
  accentColor?: string; /* Color del indicador "ON" (ej: Lima) */
  errorColor?: string;  /* Color del indicador "OFF" (ej: Rojo) */
  borderWidth?: string; /* Grosor del borde */
  shadowSize?: string;  /* Tamaño de la sombra dura */
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'ACTIVE',
  offText: 'IDLE',
  bgIdle: '#FACC15',
  bgActive: '#22d3ee',
  accentColor: '#A3E635',
  errorColor: '#ff5c5c',
  borderWidth: '3px',
  shadowSize: '4px'
});

defineEmits(['update:modelValue']);

const brutalStyles = computed(() => ({
  '--b-bg-idle': props.bgIdle,
  '--b-bg-active': props.bgActive,
  '--b-accent': props.accentColor,
  '--b-error': props.errorColor,
  '--b-border': props.borderWidth,
  '--b-shadow': props.shadowSize,
}));
</script>

<style module>
.brutalRoot {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: var(--b-bg-idle);
  border: var(--b-border) solid #000;
  box-shadow: var(--b-shadow) var(--b-shadow) 0px #000;
  cursor: pointer;
  transition: all 0.1s cubic-bezier(0.17, 0.67, 0.83, 0.67);
  outline: none;
  width: fit-content;
  user-select: none;
}

/* Efecto de elevación Neobrutalista */
.brutalRoot:hover {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--b-shadow) + 2px) calc(var(--b-shadow) + 2px) 0px #000;
}

.brutalRoot:active {
  transform: translate(var(--b-shadow), var(--b-shadow));
  box-shadow: 0px 0px 0px #000;
}

.cell {
  background: #fff;
  padding: 8px 16px;
  border: var(--b-border) solid #000;
  display: flex;
  align-items: center;
  gap: 12px;
  border-radius: 2px;
}

.label {
  font-family: 'Arial Black', 'Impact', sans-serif;
  font-size: 0.85rem;
  font-weight: 900;
  color: #000;
  text-transform: uppercase;
  letter-spacing: -0.5px;
}

.indicator {
  width: 14px;
  height: 14px;
  border: 2px solid #000;
  transition: background 0.1s ease;
}

.bgOn { background: var(--b-accent); }
.bgOff { background: var(--b-error); }

.toggleCell {
  min-width: 110px;
  justify-content: space-between;
}

.statusText {
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.8rem;
  font-weight: 900;
  color: #000;
}

.track {
  width: 44px;
  height: 24px;
  background: #000;
  position: relative;
  border-radius: 0px;
}

.thumb {
  width: 18px;
  height: 18px;
  background: #fff;
  border: 2px solid #000;
  position: absolute;
  top: 1px;
  left: 2px;
  transition: transform 0.15s cubic-bezier(0.23, 1, 0.32, 1);
}

/* --- ESTADO ACTIVO --- */

.isActive {
  background: var(--b-bg-active);
}

.isActive .thumb {
  transform: translateX(20px);
  background: var(--b-accent);
}
</style>