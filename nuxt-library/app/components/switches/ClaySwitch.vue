<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.clayIndicator, modelValue ? $style.bgOn : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.controlCell]">
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
      <span :class="$style.statusText">{{ modelValue ? 'ON' : 'OFF' }}</span>
    </div>
  </button>
</template>

<script setup lang="ts">
defineProps<{
  modelValue: boolean;
  label: string;
}>();

defineEmits(['update:modelValue']);
</script>

<style module>
:root {
  --clay-bg: #f0f3f9;
  --clay-accent: #6366f1;
  --clay-radius: 16px;
  /* Sombras exteriores (elevación) y sombras internas (volumen) */
  --clay-shadow-out: 8px 8px 16px rgba(0, 0, 0, 0.08), 
                    -8px -8px 16px rgba(255, 255, 255, 0.8);
  --clay-shadow-in: inset 4px 4px 8px rgba(0, 0, 0, 0.04), 
                   inset -4px -4px 8px rgba(255, 255, 255, 0.7);
}

.switchContainer {
  display: flex;
  gap: 12px;
  padding: 12px;
  background: var(--clay-bg);
  border-radius: calc(var(--clay-radius) * 1.5);
  border: none;
  cursor: pointer;
  box-shadow: var(--clay-shadow-out);
  transition: all 0.3s ease;
  outline: none;
}

.switchContainer:active {
  transform: scale(0.97);
  box-shadow: 4px 4px 8px rgba(0, 0, 0, 0.08);
}

.cell {
  background: var(--clay-bg);
  padding: 10px 16px;
  border-radius: var(--clay-radius);
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: var(--clay-shadow-out), var(--clay-shadow-in);
}

.label {
  font-family: 'Inter', sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  color: #64748b;
  text-transform: uppercase;
}

.clayIndicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.bgOn { 
  background: #22c55e; 
  box-shadow: inset 2px 2px 4px rgba(0,0,0,0.2), 0 0 10px rgba(34, 197, 94, 0.4);
}
.bgOff { 
  background: #cbd5e1; 
  box-shadow: var(--clay-shadow-in);
}

.controlCell {
  min-width: 90px;
  justify-content: space-between;
}

.track {
  width: 38px;
  height: 20px;
  background: #e2e8f0;
  border-radius: 10px;
  position: relative;
  box-shadow: inset 3px 3px 6px rgba(0, 0, 0, 0.1), 
              inset -3px -3px 6px rgba(255, 255, 255, 0.5);
}

.thumb {
  width: 14px;
  height: 14px;
  background: var(--clay-bg);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  box-shadow: 4px 4px 8px rgba(0, 0, 0, 0.1), 
              inset 2px 2px 4px rgba(255, 255, 255, 1);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 800;
  color: #94a3b8;
}

/* Estado Activo */
.isActive .label { color: var(--clay-accent); }

.isActive .track {
  background: var(--clay-accent);
  box-shadow: inset 3px 3px 6px rgba(0, 0, 0, 0.2);
}

.isActive .thumb {
  transform: translateX(18px);
  background: #fff;
}

.isActive .statusText { color: var(--clay-accent); }
</style>