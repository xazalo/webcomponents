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
        {{ modelValue ? 'ON' : 'OFF' }}
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
.switchContainer {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  outline: none;
  width: fit-content;
}

.switchContainer:hover {
  background: rgba(255, 255, 255, 0.9);
  transform: translateY(-1px);
}

.cell {
  background: #ffffff;
  padding: 8px 14px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(0, 0, 0, 0.03);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

.label {
  font-size: 0.75rem;
  font-weight: 700;
  color: #4b5563;
  letter-spacing: 0.02em;
}

.indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.bgSuccess { 
  background: #10b981; 
  box-shadow: 0 0 10px rgba(16, 185, 129, 0.5); 
}

.bgOff { background: #d1d5db; }

/* Estilos del Toggle */
.toggleCell {
  background: #f9fafb;
  min-width: 80px;
  justify-content: space-between;
}

.statusText {
  font-size: 0.7rem;
  font-weight: 800;
  color: #9ca3af;
  transition: color 0.3s ease;
}

.track {
  width: 32px;
  height: 18px;
  background: #e5e7eb;
  border-radius: 10px;
  position: relative;
  transition: background 0.3s ease;
}

.thumb {
  width: 14px;
  height: 14px;
  background: white;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  left: 2px;
  transition: transform 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Estado Activo */
.isActive .toggleCell {
  background: #4f46e5; /* Color acento */
}

.isActive .statusText {
  color: rgba(255, 255, 255, 0.9);
}

.isActive .track {
  background: rgba(255, 255, 255, 0.3);
}

.isActive .thumb {
  transform: translateX(14px);
}
</style>