<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.speedIndicator, modelValue ? $style.bgNitro : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.controlCell]">
      <div :class="$style.track">
        <div :class="$style.thumb">
          <div :class="$style.stripe"></div>
        </div>
      </div>
      <span :class="$style.statusText">{{ modelValue ? 'MAX_RPM' : 'IDLE' }}</span>
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
  --speed-black: #0a0a0b;
  --speed-red: #ff0000;
  --speed-white: #f1f1f1;
  --speed-italic: skewX(-12deg); /* El alma de la velocidad */
}

.switchContainer {
  display: flex;
  gap: 4px;
  padding: 6px;
  background: var(--speed-black);
  /* Degradado sutil tipo fibra de carbono */
  background-image: 
    linear-gradient(45deg, #111 25%, transparent 25%), 
    linear-gradient(-45deg, #111 25%, transparent 25%), 
    linear-gradient(45deg, transparent 75%, #111 75%), 
    linear-gradient(-45deg, transparent 75%, #111 75%);
  background-size: 4px 4px;
  
  border-left: 4px solid var(--speed-red);
  cursor: pointer;
  transition: transform 0.2s ease;
  width: fit-content;
  outline: none;
  border-top: none; border-right: none; border-bottom: none;
}

.switchContainer:hover {
  transform: scale(1.02) skewX(-2deg);
}

.cell {
  background: #1a1a1c;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 10px;
  transform: var(--speed-italic); /* Todo inclinado */
  border: 1px solid #333;
  position: relative;
  overflow: hidden;
}

.cell::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 50%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.05), transparent);
  transition: 0.5s;
}

.switchContainer:hover .cell::after {
  left: 200%;
}

.label {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.7rem;
  font-weight: 900;
  color: var(--speed-white);
  font-style: italic;
  letter-spacing: -0.5px;
}

.speedIndicator {
  width: 4px;
  height: 12px;
  background: #333;
  transform: skewX(10deg); /* Compensar la inclinación */
}

.bgNitro { 
  background: var(--speed-red); 
  box-shadow: 0 0 10px var(--speed-red);
}

.bgOff { background: #444; }

.controlCell {
  min-width: 110px;
  justify-content: space-between;
  background: #222;
}

.track {
  width: 45px;
  height: 14px;
  background: #000;
  border-radius: 2px;
  position: relative;
  overflow: hidden;
  border: 1px solid #444;
}

.thumb {
  width: 20px;
  height: 100%;
  background: #555;
  position: absolute;
  top: 0;
  left: 0;
  transition: all 0.15s cubic-bezier(0.23, 1, 0.32, 1);
  display: flex;
  justify-content: center;
  align-items: center;
}

.stripe {
  width: 2px;
  height: 60%;
  background: rgba(255,255,255,0.2);
}

.statusText {
  font-family: 'Inter', sans-serif;
  font-size: 0.6rem;
  font-weight: 900;
  color: #555;
  font-style: italic;
}

/* Estado Activo: Aceleración */
.isActive .cell {
  border-color: var(--speed-red);
}

.isActive .thumb {
  transform: translateX(25px);
  background: var(--speed-red);
}

.isActive .statusText {
  color: var(--speed-red);
  text-shadow: 0 0 5px rgba(255,0,0,0.5);
}

.isActive .track {
  background: linear-gradient(90deg, #300, #000);
}
</style>