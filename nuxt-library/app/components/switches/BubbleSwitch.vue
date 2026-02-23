<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.bubbleRoot, { [$style.isActive]: modelValue }]"
    :style="bubbleStyles"
    @click="$emit('update:modelValue', !modelValue)"
  >
    <div :class="$style.cell">
      <div :class="[$style.bubbleIndicator, modelValue ? $style.bgActive : $style.bgOff]">
        <div :class="$style.miniGlare"></div>
      </div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div :class="[$style.cell, $style.toggleCell]">
      <div :class="$style.track">
        <div :class="$style.thumb">
          <div :class="$style.glare"></div>
        </div>
      </div>
      <span :class="$style.statusText">
        {{ modelValue ? onText : offText }}
      </span>
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
  // Bubble Tuning
  colorIdle?: string;    /* Color cuando está OFF (Rosa/Gris) */
  colorActive?: string;  /* Color cuando está ON (Cyan/Verde) */
  accentText?: string;   /* Color del texto */
  radius?: string;       /* Nivel de redondez */
  bounce?: number;       /* Escala de rebote al click (0.9 a 1.1) */
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'YAY!',
  offText: 'NOPE',
  colorIdle: '#ff8dfb',
  colorActive: '#70e0ff',
  accentText: '#ff70e0',
  radius: '40px',
  bounce: 0.95
});

defineEmits(['update:modelValue']);

const bubbleStyles = computed(() => ({
  '--b-main': props.colorIdle,
  '--b-active': props.colorActive,
  '--b-text': props.accentText,
  '--b-radius': props.radius,
  '--b-bounce': props.bounce,
}));
</script>

<style module>
.bubbleRoot {
  display: flex;
  gap: 10px;
  padding: 8px;
  background: color-mix(in srgb, var(--b-main) 10%, #fff);
  border-radius: var(--b-radius);
  border: none;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  width: fit-content;
  outline: none;
}

.bubbleRoot:hover {
  transform: scale(1.02);
  background: #fff;
}

.bubbleRoot:active {
  transform: scale(var(--b-bounce));
}

.cell {
  background: #fff;
  padding: 8px 16px;
  border-radius: var(--b-radius);
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: inset 2px 4px 8px rgba(255, 255, 255, 0.8), 
              inset -2px -4px 8px rgba(0, 0, 0, 0.03);
}

.label {
  font-family: 'Rounded Mplus 1c', sans-serif;
  font-size: 0.8rem;
  font-weight: 800;
  color: var(--b-text);
  transition: color 0.3s ease;
}

/* Indicador de estado con volumen */
.bubbleIndicator {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  position: relative;
  transition: all 0.4s ease;
  box-shadow: inset -2px -2px 4px rgba(0,0,0,0.1);
}

.miniGlare {
  position: absolute;
  top: 2px; left: 3px;
  width: 4px; height: 3px;
  background: rgba(255,255,255,0.6);
  border-radius: 50%;
}

.bgActive { background: var(--b-active); box-shadow: 0 0 10px var(--b-active); }
.bgOff { background: #e2e8f0; }

.toggleCell {
  gap: 10px;
  background: #fff;
}

.track {
  width: 46px;
  height: 24px;
  background: #f1f5f9;
  border-radius: 20px;
  position: relative;
  box-shadow: inset 2px 3px 6px rgba(0,0,0,0.06);
}

.thumb {
  width: 18px;
  height: 18px;
  background: var(--b-main);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: transform 0.5s cubic-bezier(0.68, -0.6, 0.3, 1.6);
  box-shadow: 0 4px 8px color-mix(in srgb, var(--b-main), transparent 50%);
}

.glare {
  width: 6px; height: 4px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  position: absolute;
  top: 3px; left: 4px;
}

.statusText {
  font-size: 0.7rem;
  font-weight: 900;
  color: #94a3b8;
  letter-spacing: 0.05em;
}

/* --- ESTADO ACTIVO --- */

.isActive {
  background: color-mix(in srgb, var(--b-active) 10%, #fff);
}

.isActive .label {
  color: color-mix(in srgb, var(--b-active) 80%, #000);
}

.isActive .thumb {
  transform: translateX(22px);
  background: var(--b-active);
  box-shadow: 0 4px 8px color-mix(in srgb, var(--b-active), transparent 50%);
}

.isActive .statusText {
  color: var(--b-active);
  filter: brightness(0.8);
}
</style>