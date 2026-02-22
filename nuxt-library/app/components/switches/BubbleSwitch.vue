<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.bubbleIndicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <div :class="$style.track">
        <div :class="$style.thumb">
          <div :class="$style.glare"></div>
        </div>
      </div>
      <span :class="$style.statusText">
        {{ modelValue ? 'YAY!' : 'NOPE' }}
      </span>
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
  --bubble-radius: 40px;
  --color-main: #ff8dfb; /* Rosa chicle */
  --color-active: #70e0ff; /* Azul cielo */
  --soft-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  --inner-glare: inset 2px 4px 8px rgba(255, 255, 255, 0.8), 
                 inset -2px -4px 8px rgba(0, 0, 0, 0.05);
}

.switchContainer {
  display: flex;
  gap: 10px;
  padding: 10px;
  background: #fdf2f8;
  border-radius: var(--bubble-radius);
  border: none;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: var(--soft-shadow);
  width: fit-content;
  outline: none;
}

.switchContainer:hover {
  transform: scale(1.03);
  background: #ffffff;
}

.switchContainer:active {
  transform: scale(0.96);
}

.cell {
  background: #ffffff;
  padding: 10px 18px;
  border-radius: var(--bubble-radius);
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: var(--inner-glare);
}

.label {
  font-family: 'Rounded Mplus 1c', 'Comic Sans MS', sans-serif;
  font-size: 0.8rem;
  font-weight: 800;
  color: #ff70e0;
}

.bubbleIndicator {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  box-shadow: inset -2px -2px 4px rgba(0,0,0,0.1);
}

.bgActive { background: var(--color-active); }
.bgOff { background: #e2e8f0; }

.toggleCell {
  gap: 10px;
  background: #fff;
}

.track {
  width: 44px;
  height: 24px;
  background: #eeeef2;
  border-radius: 20px;
  position: relative;
  box-shadow: inset 2px 2px 5px rgba(0,0,0,0.05);
}

.thumb {
  width: 18px;
  height: 18px;
  background: var(--color-main);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  box-shadow: 2px 4px 6px rgba(255, 141, 251, 0.4);
}

.glare {
  width: 6px;
  height: 4px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 4px;
}

.statusText {
  font-size: 0.7rem;
  font-weight: 900;
  color: #94a3b8;
  letter-spacing: 0.5px;
}

/* Cambios en estado Activo */
.isActive {
  background: #eefbff;
}

.isActive .label {
  color: #38bdf8;
}

.isActive .thumb {
  transform: translateX(20px);
  background: var(--color-active);
  box-shadow: 2px 4px 6px rgba(112, 224, 255, 0.4);
}

.isActive .statusText {
  color: #0ea5e9;
}
</style>