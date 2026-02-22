<template>
  <button 
    :class="[$style.switchContainer, { [$style.isActive]: modelValue }]"
    @click="$emit('update:modelValue', !modelValue)"
    type="button"
  >
    <div :class="$style.cell">
      <div :class="[$style.glassIndicator, modelValue ? $style.bgActive : $style.bgOff]"></div>
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
.switchContainer {
  display: flex;
  gap: 8px;
  padding: 8px;
  /* El fondo base del cristal */
  background: rgba(255, 255, 255, 0.1);
  /* El efecto de desenfoque es vital */
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
  width: fit-content;
  outline: none;
}

.switchContainer:hover {
  background: rgba(255, 255, 255, 0.15);
  border-color: rgba(255, 255, 255, 0.3);
  transform: translateY(-1px);
}

.cell {
  /* Celdas con un blanco muy sutil para destacar del contenedor */
  background: rgba(255, 255, 255, 0.05);
  padding: 8px 14px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  gap: 10px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.label {
  font-family: 'Inter', sans-serif;
  font-size: 0.75rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  letter-spacing: 0.02em;
}

.glassIndicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.bgActive { 
  background: #34d399; 
  box-shadow: 0 0 12px #34d399;
}
.bgOff { background: rgba(255, 255, 255, 0.3); }

.toggleCell {
  min-width: 85px;
  justify-content: space-between;
  background: rgba(255, 255, 255, 0.08);
}

.track {
  width: 34px;
  height: 18px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 20px;
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.thumb {
  width: 14px;
  height: 14px;
  background: #ffffff;
  border-radius: 50%;
  position: absolute;
  top: 1px;
  left: 2px;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.statusText {
  font-size: 0.7rem;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.6);
}

/* Estado Activo */
.isActive {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.4);
}

.isActive .thumb {
  transform: translateX(14px);
  background: #ffffff;
}

.isActive .statusText {
  color: #ffffff;
}

.isActive .track {
  background: rgba(52, 211, 153, 0.3);
  border-color: rgba(52, 211, 153, 0.5);
}
</style>