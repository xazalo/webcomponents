<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.glassRoot, { [$style.isActive]: modelValue }]"
    :style="glassStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.glassIndicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
      <span :class="$style.statusText">{{ modelValue ? onText : offText }}</span>
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
  // Glass Tuning
  accentColor?: string;   /* Color cuando está ON (ej: Esmeralda) */
  blurAmount?: string;    /* Intensidad del desenfoque (ej: 12px) */
  glassOpacity?: number;  /* Opacidad del fondo (0 a 1) */
  tintColor?: string;     /* Tinte del cristal (blanco o negro) */
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'ON',
  offText: 'OFF',
  accentColor: '#34d399',
  blurAmount: '12px',
  glassOpacity: 0.1,
  tintColor: '255, 255, 255', // Formato RGB
  borderRadius: '16px'
});

defineEmits(['update:modelValue']);

const glassStyles = computed(() => ({
  '--g-accent': props.accentColor,
  '--g-blur': props.blurAmount,
  '--g-bg-opacity': props.glassOpacity,
  '--g-tint': props.tintColor,
  '--g-radius': props.borderRadius,
}));
</script>

<style module>
.glassRoot {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: rgba(var(--g-tint), var(--g-bg-opacity));
  
  /* Efectos de cristal esenciales */
  backdrop-filter: blur(var(--g-blur)) saturate(180%);
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(180%);
  
  border-radius: var(--g-radius);
  border: 1px solid rgba(var(--g-tint), 0.2);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  width: fit-content;
  outline: none;
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
}

.glassRoot:hover {
  background: rgba(var(--g-tint), calc(var(--g-bg-opacity) + 0.05));
  border-color: rgba(var(--g-tint), 0.4);
  transform: translateY(-1px);
}

.cell {
  background: rgba(var(--g-tint), 0.05);
  padding: 8px 14px;
  border-radius: calc(var(--g-radius) - 4px);
  display: flex;
  align-items: center;
  gap: 10px;
  border: 1px solid rgba(var(--g-tint), 0.1);
  transition: all 0.3s ease;
}

.label {
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 0.75rem;
  font-weight: 600;
  color: rgba(var(--g-tint), 0.9);
  letter-spacing: 0.02em;
}

.glassIndicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.bgActive { 
  background: var(--g-accent); 
  box-shadow: 0 0 15px var(--g-accent);
}

.bgOff { 
  background: rgba(var(--g-tint), 0.3); 
}

.toggleCell {
  min-width: 90px;
  justify-content: space-between;
  background: rgba(var(--g-tint), 0.08);
}

.track {
  width: 36px;
  height: 18px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 20px;
  position: relative;
  border: 1px solid rgba(var(--g-tint), 0.1);
  transition: all 0.3s ease;
}

.thumb {
  width: 14px;
  height: 14px;
  background: #ffffff;
  border-radius: 50%;
  position: absolute;
  top: 1px;
  left: 2px;
  transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 800;
  color: rgba(var(--g-tint), 0.5);
  transition: all 0.3s ease;
}

/* --- ESTADO ACTIVO --- */

.isActive {
  background: rgba(var(--g-tint), calc(var(--g-bg-opacity) + 0.1));
  border-color: rgba(var(--g-tint), 0.5);
}

.isActive .thumb {
  transform: translateX(16px);
  background: #ffffff;
}

.isActive .statusText {
  color: rgba(var(--g-tint), 1);
  text-shadow: 0 0 8px rgba(var(--g-tint), 0.5);
}

.isActive .track {
  background: color-mix(in srgb, var(--g-accent), transparent 70%);
  border-color: var(--g-accent);
}
</style>