<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.switchRoot, { [$style.isActive]: modelValue }]"
    :style="switchStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.indicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
      <div :class="$style.stack">
        <span :class="$style.label">{{ label }}</span>
        <span v-if="subLabel" :class="$style.subLabel">{{ subLabel }}</span>
      </div>
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
  subLabel?: string;
  onText?: string;
  offText?: string;
  // Tuning de estilo
  activeColor?: string;
  indicatorColor?: string;
  borderRadius?: string;
  width?: string;
}

const props = withDefaults(defineProps<Props>(), {
  subLabel: '',
  onText: 'ON',
  offText: 'OFF',
  activeColor: '#4f46e5',
  indicatorColor: '#10b981',
  borderRadius: '20px',
  width: 'fit-content'
});

defineEmits(['update:modelValue']);

const switchStyles = computed(() => ({
  '--s-accent': props.activeColor,
  '--s-success': props.indicatorColor,
  '--s-radius': props.borderRadius,
  '--s-width': props.width
}));
</script>

<style module>
.switchRoot {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: var(--s-radius);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  outline: none;
  width: var(--s-width);
  border-bottom: 2px solid rgba(0, 0, 0, 0.1); /* Efecto de profundidad */
}

.switchRoot:hover {
  background: rgba(255, 255, 255, 0.85);
  transform: translateY(-1px);
}

.switchRoot:active {
  transform: translateY(1px);
  border-bottom-width: 0px;
}

.cell {
  background: #ffffff;
  padding: 8px 14px;
  border-radius: calc(var(--s-radius) - 6px);
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(0, 0, 0, 0.03);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

.stack {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  line-height: 1.2;
}

.label {
  font-size: 0.75rem;
  font-weight: 800;
  color: #1f2937;
  letter-spacing: 0.01em;
}

.subLabel {
  font-size: 0.6rem;
  color: #9ca3af;
  font-weight: 600;
}

.indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.bgActive { 
  background: var(--s-success); 
  box-shadow: 0 0 12px var(--s-success); 
  transform: scale(1.2);
}

.bgOff { background: #d1d5db; }

/* Celda Toggle */
.toggleCell {
  background: #f9fafb;
  min-width: 90px;
  justify-content: space-between;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 900;
  color: #9ca3af;
  letter-spacing: 0.05em;
  transition: all 0.3s ease;
}

.track {
  width: 34px;
  height: 18px;
  background: #e5e7eb;
  border-radius: 10px;
  position: relative;
  transition: all 0.3s ease;
}

.thumb {
  width: 14px;
  height: 14px;
  background: white;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  left: 2px;
  /* El secreto del rebote Apple-style */
  transition: transform 0.4s cubic-bezier(0.68, -0.4, 0.265, 1.4);
  box-shadow: 0 2px 4px rgba(0,0,0,0.15);
}

/* --- ESTADOS ACTIVOS --- */

.isActive .toggleCell {
  background: var(--s-accent);
  border-color: rgba(0,0,0,0.1);
}

.isActive .statusText {
  color: #ffffff;
}

.isActive .track {
  background: rgba(255, 255, 255, 0.3);
}

.isActive .thumb {
  transform: translateX(16px);
}
</style>