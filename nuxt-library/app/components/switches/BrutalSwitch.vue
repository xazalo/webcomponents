<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.indicator, modelValue ? $style.bgSuccess : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <span :class="$style.statusText">
        {{ modelValue ? 'ACTIVE' : 'IDLE' }}
      </span>
      <div :class="$style.track">
        <div :class="$style.thumb"></div>
      </div>
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
/* Paleta Neobrutalista: Bordes negros gruesos y sombras sin blur */
:root {
  --border-width: 3px;
  --shadow-offset: 4px;
  --color-black: #000000;
  --color-white: #ffffff;
  --color-accent: #A3E635; /* Lima Neobrutal */
  --color-bg: #FACC15;     /* Amarillo Neobrutal */
}

.switchContainer {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: var(--color-bg);
  border: var(--border-width) solid var(--color-black);
  box-shadow: var(--shadow-offset) var(--shadow-offset) 0px var(--color-black);
  cursor: pointer;
  transition: all 0.1s active;
  outline: none;
  width: fit-content;
}

.switchContainer:hover {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--shadow-offset) + 2px) calc(var(--shadow-offset) + 2px) 0px var(--color-black);
}

.switchContainer:active {
  transform: translate(2px, 2px);
  box-shadow: 0px 0px 0px var(--color-black);
}

.cell {
  background: var(--color-white);
  padding: 8px 16px;
  border: var(--border-width) solid var(--color-black);
  display: flex;
  align-items: center;
  gap: 12px;
  /* El bento neobrutal no suele llevar border-radius o es muy pequeño */
  border-radius: 4px;
}

.label {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.8rem;
  font-weight: 900;
  color: var(--color-black);
  text-transform: uppercase;
}

.indicator {
  width: 12px;
  height: 12px;
  border: 2px solid var(--color-black);
}

.bgSuccess { background: var(--color-accent); }
.bgOff { background: #ff5c5c; } /* Rojo neobrutal */

.toggleCell {
  background: var(--color-white);
  min-width: 100px;
  justify-content: space-between;
}

.statusText {
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.75rem;
  font-weight: 900;
  color: var(--color-black);
}

.track {
  width: 40px;
  height: 22px;
  background: var(--color-black);
  position: relative;
  border-radius: 0px;
}

.thumb {
  width: 16px;
  height: 16px;
  background: var(--color-white);
  border: 2px solid var(--color-black);
  position: absolute;
  top: 1px;
  left: 2px;
  transition: transform 0.15s ease-out;
}

/* Variación Activa */
.isActive {
  background: #22d3ee; /* Cyan en lugar de amarillo al activar */
}

.isActive .thumb {
  transform: translateX(18px);
  background: var(--color-accent);
}

.isActive .toggleCell {
  background: var(--color-white);
}
</style>