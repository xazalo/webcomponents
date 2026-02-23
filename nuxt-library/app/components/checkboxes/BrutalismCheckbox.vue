<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.brutalWrapper, 
      $style[size], 
      { [$style.checked]: modelValue }
    ]" 
    :style="brutalStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.brutalBox">
        <svg v-if="modelValue" viewBox="0 0 24 24" :class="$style.checkIcon">
          <path d="M20 6L9 17L4 12" fill="none" stroke="currentColor" :stroke-width="iconStroke" stroke-linecap="square" stroke-linejoin="square"/>
        </svg>
      </div>
    </div>
    <span v-if="label" :class="$style.label">{{ label }}</span>
  </label>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  label?: string;
  // Colores
  accentColor?: string;   /* Fondo del box cuando está marcado */
  baseColor?: string;     /* Fondo del botón entero */
  textColor?: string;     /* Color de letra y box desmarcado */
  borderColor?: string;   /* Color de trazos y sombras */
  // Estructura
  size?: 'sm' | 'md' | 'lg';
  borderWidth?: string;   /* Grosor de las líneas */
  shadowSize?: string;    /* Distancia de la sombra sólida */
  borderRadius?: string;  /* Curvatura (recomiendo 0 o poca) */
  iconStroke?: number;    /* Grosor del check */
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  accentColor: '#00ff41',
  baseColor: '#ffffff',
  textColor: '#000000',
  borderColor: '#000000',
  size: 'md',
  borderWidth: '3px',
  shadowSize: '4px',
  borderRadius: '0px',
  iconStroke: 4
});

defineEmits(['update:modelValue']);

const brutalStyles = computed(() => ({
  '--brutal-accent': props.accentColor,
  '--brutal-bg': props.baseColor,
  '--brutal-text': props.textColor,
  '--brutal-border': props.borderColor,
  '--brutal-bw': props.borderWidth,
  '--brutal-sw': props.shadowSize,
  '--brutal-radius': props.borderRadius,
}));
</script>

<style module>
.brutalWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  padding: 0.6rem 1rem;
  background: var(--brutal-bg);
  border: var(--brutal-bw) solid var(--brutal-border);
  border-radius: var(--brutal-radius);
  box-shadow: var(--brutal-sw) var(--brutal-sw) 0px 0px var(--brutal-border);
  transition: all 0.1s cubic-bezier(0.17, 0.67, 0.83, 0.67);
  user-select: none;
}

.brutalWrapper:hover {
  transform: translate(calc(var(--brutal-sw) * -0.5), calc(var(--brutal-sw) * -0.5));
  box-shadow: calc(var(--brutal-sw) * 1.5) calc(var(--brutal-sw) * 1.5) 0px 0px var(--brutal-border);
}

.brutalWrapper:active {
  transform: translate(var(--brutal-sw), var(--brutal-sw));
  box-shadow: 0px 0px 0px 0px var(--brutal-border);
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

.brutalBox {
  width: 1.5em;
  height: 1.5em;
  background: var(--brutal-bg);
  border: calc(var(--brutal-bw) * 0.7) solid var(--brutal-border);
  border-radius: calc(var(--brutal-radius) * 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.1s;
}

/* Estado Marcado */
.checked .brutalBox {
  background: var(--brutal-accent);
}

.checked {
  /* Opcional: El wrapper puede cambiar sutilmente al estar activo */
  background: #fdfdfd; 
}

.checkIcon {
  color: var(--brutal-border);
  width: 80%;
  height: 80%;
}

.label {
  font-size: 1rem;
  font-weight: 900;
  text-transform: uppercase;
  color: var(--brutal-text);
  letter-spacing: 0.02em;
}

/* Tamaños */
.sm { font-size: 0.75rem; padding: 0.4rem 0.7rem; --brutal-sw: 2px; }
.md { font-size: 1rem; padding: 0.6rem 1rem; }
.lg { font-size: 1.25rem; padding: 0.8rem 1.4rem; --brutal-sw: 6px; }

@media (prefers-reduced-motion: reduce) {
  .brutalWrapper { transition: none; }
}
</style>