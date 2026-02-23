<template>
  <button 
    v-bind="$attrs"
    type="button"
    :class="[$style.speedRoot, { [$style.isActive]: modelValue }]"
    :style="speedStyles"
    @click="$emit('update:modelValue', !modelValue)"
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
  // Racing Tuning
  nitroColor?: string;    /* Color de activación (Rojo/Fuego) */
  baseColor?: string;     /* Fondo de celdas (Carbon/Oscuro) */
  italicAngle?: number;   /* Grados de inclinación (Sugerido: -12) */
  showCarbon?: boolean;   /* ¿Patrón de fibra de carbono? */
  accentBorder?: string;  /* Color del borde lateral de énfasis */
}

const props = withDefaults(defineProps<Props>(), {
  onText: 'MAX_RPM',
  offText: 'IDLE',
  nitroColor: '#ff0000',
  baseColor: '#1a1a1c',
  italicAngle: -12,
  showCarbon: true,
  accentBorder: '#ff0000'
});

defineEmits(['update:modelValue']);

const speedStyles = computed(() => ({
  '--s-nitro': props.nitroColor,
  '--s-bg-cell': props.baseColor,
  '--s-skew': `${props.italicAngle}deg`,
  '--s-skew-inv': `${props.italicAngle * -1}deg`,
  '--s-accent-line': props.accentBorder,
  '--s-carbon-display': props.showCarbon ? 'block' : 'none',
}));
</script>

<style module>
.speedRoot {
  display: flex;
  gap: 6px;
  padding: 8px;
  background: #0a0a0b;
  
  /* Patrón Fibra de Carbono dinámico */
  background-image: linear-gradient(45deg, #111 25%, transparent 25%), 
                    linear-gradient(-45deg, #111 25%, transparent 25%), 
                    linear-gradient(45deg, transparent 75%, #111 75%), 
                    linear-gradient(-45deg, transparent 75%, #111 75%);
  background-size: 4px 4px;
  
  border-left: 4px solid var(--s-accent-line);
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  width: fit-content;
  outline: none;
  border-top: none; border-right: none; border-bottom: none;
  user-select: none;
}

.speedRoot:hover {
  transform: scale(1.02) translateX(4px);
  filter: brightness(1.2);
}

.speedRoot:active {
  transform: scale(0.98);
}

.cell {
  background: var(--s-bg-cell);
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  transform: skewX(var(--s-skew)); /* El alma del componente */
  border: 1px solid #333;
  position: relative;
  overflow: hidden;
  transition: border-color 0.3s ease;
}

/* Efecto Reflejo de Velocidad */
.cell::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 50%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.08), transparent);
  transition: 0.6s;
}

.speedRoot:hover .cell::after {
  left: 200%;
}

.label {
  font-family: 'Arial Black', 'Impact', sans-serif;
  font-size: 0.75rem;
  font-weight: 900;
  color: #f1f1f1;
  font-style: italic;
  letter-spacing: -0.5px;
  /* Compensamos la lectura del texto para que no sea excesivamente difícil */
  transform: skewX(calc(var(--s-skew-inv) / 2));
}

.speedIndicator {
  width: 5px;
  height: 14px;
  background: #333;
  transform: skewX(calc(var(--s-skew-inv))); 
  transition: all 0.3s ease;
}

.bgNitro { 
  background: var(--s-nitro); 
  box-shadow: 0 0 12px var(--s-nitro);
}

.bgOff { background: #444; }

.controlCell {
  min-width: 130px;
  justify-content: space-between;
  background: color-mix(in srgb, var(--s-bg-cell), #fff 5%);
}

.track {
  width: 48px;
  height: 16px;
  background: #000;
  border-radius: 2px;
  position: relative;
  overflow: hidden;
  border: 1px solid #444;
}

.thumb {
  width: 22px;
  height: 100%;
  background: #444;
  position: absolute;
  top: 0; left: 0;
  transition: all 0.2s cubic-bezier(0.19, 1, 0.22, 1);
  display: flex;
  justify-content: center;
  align-items: center;
}

.stripe {
  width: 2px;
  height: 60%;
  background: rgba(255,255,255,0.3);
  box-shadow: 4px 0 0 rgba(255,255,255,0.1), -4px 0 0 rgba(255,255,255,0.1);
}

.statusText {
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 0.65rem;
  font-weight: 900;
  color: #666;
  font-style: italic;
  letter-spacing: 0.5px;
  transition: color 0.3s ease;
}

/* --- ESTADO NITRO (ACTIVO) --- */

.isActive .cell {
  border-color: var(--s-nitro);
}

.isActive .thumb {
  transform: translateX(26px);
  background: var(--s-nitro);
  box-shadow: -5px 0 15px color-mix(in srgb, var(--s-nitro), transparent 40%);
}

.isActive .statusText {
  color: var(--s-nitro);
  text-shadow: 0 0 8px color-mix(in srgb, var(--s-nitro), transparent 50%);
}

.isActive .track {
  background: linear-gradient(90deg, color-mix(in srgb, var(--s-nitro), #000 70%), #000);
  border-color: color-mix(in srgb, var(--s-nitro), transparent 60%);
}
</style>