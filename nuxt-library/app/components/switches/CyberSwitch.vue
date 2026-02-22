<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.scanline]"></div>
      <div :class="[$style.cyberIndicator, modelValue ? $style.bgCyber : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.controlCell]">
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
      <span :class="$style.statusText">{{ modelValue ? 'LINK_ESTABLISHED' : 'DISCONNECTED' }}</span>
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
  --cb-black: #050505;
  --cb-yellow: #fcee0a;
  --cb-blue: #00f0ff;
  --cb-pink: #ff003c;
  --cb-border: 2px;
}

.switchContainer {
  display: flex;
  gap: 10px;
  padding: 8px;
  background: var(--cb-black);
  border: var(--cb-border) solid var(--cb-yellow);
  clip-path: polygon(0 0, 95% 0, 100% 30%, 100% 100%, 5% 100%, 0 70%);
  cursor: pointer;
  position: relative;
  transition: all 0.2s ease;
  width: fit-content;
}

.switchContainer:hover {
  box-shadow: 0 0 15px rgba(252, 238, 10, 0.4);
  transform: scale(1.02);
}

.cell {
  background: #111;
  padding: 10px 15px;
  border: 1px solid #333;
  display: flex;
  align-items: center;
  gap: 12px;
  position: relative;
  overflow: hidden;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 10px 100%, 0 calc(100% - 10px));
}

.scanline {
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(to bottom, transparent 50%, rgba(0, 240, 255, 0.05) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.label {
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.7rem;
  font-weight: 900;
  color: var(--cb-yellow);
  text-transform: uppercase;
  letter-spacing: 2px;
  text-shadow: 2px 2px 0px rgba(255, 0, 60, 0.5);
}

.cyberIndicator {
  width: 12px;
  height: 4px;
  background: #333;
  transition: all 0.3s cyan;
}

.bgCyber {
  background: var(--cb-blue);
  box-shadow: 0 0 10px var(--cb-blue);
}

.bgOff { background: #333; }

.controlCell {
  min-width: 140px;
  justify-content: space-between;
}

.track {
  width: 40px;
  height: 12px;
  background: #222;
  border: 1px solid #444;
  position: relative;
}

.thumb {
  width: 16px;
  height: 100%;
  background: #444;
  position: absolute;
  top: 0;
  left: 0;
  transition: all 0.2s cubic-bezier(1, 0, 0, 1);
}

.statusText {
  font-family: 'Consolas', monospace;
  font-size: 0.6rem;
  color: #666;
}

/* Estado Activo */
.isActive {
  border-color: var(--cb-blue);
}

.isActive .label {
  color: var(--cb-blue);
  text-shadow: 2px 2px 0px var(--cb-pink);
}

.isActive .track {
  border-color: var(--cb-pink);
}

.isActive .thumb {
  transform: translateX(22px);
  background: var(--cb-pink);
  box-shadow: 0 0 15px var(--cb-pink);
}

.isActive .statusText {
  color: var(--cb-pink);
  animation: blink 0.5s infinite;
}

@keyframes blink {
  50% { opacity: 0.3; }
}
</style>