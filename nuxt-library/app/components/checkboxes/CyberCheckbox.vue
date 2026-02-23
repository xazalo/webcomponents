<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.cyberWrapper, 
      $style[size], 
      { [$style.checked]: modelValue, [$style.disabled]: disabled }
    ]" 
    :style="cyberStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        :disabled="disabled"
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.cyberBox">
        <svg v-if="modelValue" viewBox="0 0 24 24" :class="$style.checkIcon">
          <path d="M20 6L9 17L4 12" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="square" stroke-linejoin="square"/>
        </svg>
      </div>
      <div v-if="modelValue" :class="$style.glitchLine" />
    </div>
    <span v-if="label" :class="$style.label">
      {{ label }}
      <span :class="$style.subLabel" v-if="modelValue">SYSTEM_READY</span>
    </span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label?: string;
  disabled?: boolean;
  // Estética Cyber
  neonColor?: string;     /* El color del neón (ej: #00f3ff) */
  baseColor?: string;     /* Fondo del componente desmarcado */
  cutSize?: string;       /* Qué tan agresivo es el corte de esquina */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  disabled: false,
  neonColor: '#00f3ff', // Cyan eléctrico por defecto
  baseColor: '#1a1a1a', // Negro carbón
  cutSize: '8px',
  size: 'md'
});

defineEmits(['update:modelValue']);

const cyberStyles = computed(() => ({
  '--c-neon': props.neonColor,
  '--c-bg': props.baseColor,
  '--c-cut': props.cutSize,
  '--c-glow': `0 0 15px ${props.neonColor}88`,
}));
</script>

<style module>
.cyberWrapper {
  display: inline-flex;
  align-items: center;
  gap: 1rem;
  cursor: pointer;
  user-select: none;
  transition: all 0.2s ease;
  padding: 0.5rem;
}

.inputContainer {
  position: relative;
  display: flex;
}

.inputContainer input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.cyberBox {
  width: 1.5em;
  height: 1.5em;
  background: var(--c-bg);
  /* Corte geométrico Cyberpunk clásico */
  clip-path: polygon(
    var(--c-cut) 0%, 100% 0%, 
    100% calc(100% - var(--c-cut)), 
    calc(100% - var(--c-cut)) 100%, 
    0% 100%, 0% var(--c-cut)
  );
  border: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
}

/* Estado Marcado */
.checked .cyberBox {
  background: var(--c-neon);
  box-shadow: var(--c-glow);
  transform: scale(1.05);
}

.checkIcon {
  color: #000; /* Contraste negro sobre neón */
  width: 70%;
  height: 70%;
}

.label {
  font-family: 'Courier New', Courier, monospace; /* Fuente técnica */
  font-weight: 900;
  color: #fff;
  text-transform: uppercase;
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  display: flex;
  flex-direction: column;
}

.subLabel {
  font-size: 0.6rem;
  color: var(--c-neon);
  opacity: 0.8;
  margin-top: 2px;
}

/* Efecto de línea de glitch */
.glitchLine {
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 2px;
  background: var(--c-neon);
  box-shadow: 0 0 8px var(--c-neon);
  animation: scan 1.5s infinite linear;
}

/* Tamaños */
.sm { font-size: 14px; }
.md { font-size: 20px; }
.lg { font-size: 28px; }

.disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.cyberWrapper:hover:not(.disabled) .cyberBox {
  border-color: var(--c-neon);
  box-shadow: 0 0 10px rgba(var(--c-neon), 0.2);
}

@keyframes scan {
  0% { clip-path: inset(0 100% 0 0); }
  50% { clip-path: inset(0 0 0 0); }
  100% { clip-path: inset(0 0 0 100%); }
}
</style>