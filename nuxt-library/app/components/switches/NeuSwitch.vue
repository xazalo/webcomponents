<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.neuIndicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
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
  /* El color de fondo DEBE ser el mismo que el de los elementos */
  --neu-bg: #e0e5ec;
  --neu-light: #ffffff;
  --neu-shadow: #a3b1c6;
  --neu-accent: #6d5dfc;
}

.switchContainer {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: var(--neu-bg);
  border-radius: 20px;
  border: none;
  cursor: pointer;
  /* Sombra cóncava suave para el contenedor */
  box-shadow: 6px 6px 12px var(--neu-shadow), 
             -6px -6px 12px var(--neu-light);
  transition: all 0.3s ease;
  width: fit-content;
  outline: none;
}

.switchContainer:active {
  box-shadow: inset 4px 4px 8px var(--neu-shadow), 
              inset -4px -4px 8px var(--neu-light);
}

.cell {
  background: var(--neu-bg);
  padding: 10px 16px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  gap: 12px;
  /* Sombra convexa (hacia afuera) */
  box-shadow: 4px 4px 8px var(--neu-shadow), 
             -4px -4px 8px var(--neu-light);
}

.label {
  font-family: 'Inter', sans-serif;
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
  box-shadow: inset 2px 2px 4px var(--neu-shadow), 
              inset -2px -2px 4px var(--neu-light);
}

.bgActive { background: var(--neu-accent); box-shadow: 0 0 8px var(--neu-accent); }
.bgOff { background: #d1d9e6; }

.toggleCell {
  min-width: 90px;
  justify-content: space-between;
}

.track {
  width: 40px;
  height: 20px;
  background: var(--neu-bg);
  border-radius: 10px;
  position: relative;
  /* El track está "hundido" */
  box-shadow: inset 3px 3px 6px var(--neu-shadow), 
              inset -3px -3px 6px var(--neu-light);
}

.thumb {
  width: 14px;
  height: 14px;
  background: var(--neu-bg);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  /* El thumb está "hacia afuera" */
  box-shadow: 2px 2px 4px var(--neu-shadow), 
             -2px -2px 4px var(--neu-light);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 800;
  color: #9baacf;
}

/* Variaciones Activas */
.isActive .track {
  /* Cambia el color del fondo hundido al activar */
  background: #f0f3f8;
}

.isActive .thumb {
  transform: translateX(20px);
  background: var(--neu-accent);
  box-shadow: 0 0 10px var(--neu-accent);
}

.isActive .statusText {
  color: var(--neu-accent);
}
</style>