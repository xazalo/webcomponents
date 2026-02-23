<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.neuWrapper, 
      $style[size], 
      { [$style.checked]: modelValue, [$style.disabled]: disabled }
    ]" 
    :style="neuStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        :disabled="disabled"
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.neuBox">
        <Transition name="neu-fade">
          <svg v-if="modelValue" viewBox="0 0 24 24" :class="$style.checkIcon">
            <path d="M20 6L9 17L4 12" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </Transition>
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
  disabled?: boolean;
  // Colores (Personalización Pro)
  baseColor?: string;     /* El color de tu superficie (importante) */
  accentColor?: string;   /* Color del check */
  // Parámetros Físicos
  size?: 'sm' | 'md' | 'lg';
  intensity?: number;     /* Opacidad de las sombras (0.1 a 1) */
  distance?: number;      /* Qué tanto sobresale (2 a 10) */
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  disabled: false,
  baseColor: '#e0e5ec',
  accentColor: '#4f46e5',
  size: 'md',
  intensity: 0.45,
  distance: 4
});

defineEmits(['update:modelValue']);

const neuStyles = computed(() => ({
  '--n-bg': props.baseColor,
  '--n-accent': props.accentColor,
  '--n-dist': `${props.distance}px`,
  '--n-dist-neg': `-${props.distance}px`,
  '--n-blur': `${props.distance * 2}px`,
  '--n-shad-dark': `rgba(163, 177, 198, ${props.intensity})`,
  '--n-shad-light': `rgba(255, 255, 255, ${props.intensity + 0.2})`,
}));
</script>

<style module>
.neuWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.6rem 1.1rem;
  background: var(--n-bg);
  border-radius: 1rem;
  user-select: none;
  transition: all 0.3s ease;
  /* El contenedor tiene una sombra sutil para "flotar" levemente */
  box-shadow: 
    calc(var(--n-dist) / 2) calc(var(--n-dist) / 2) var(--n-blur) var(--n-shad-dark),
    calc(var(--n-dist-neg) / 2) calc(var(--n-dist-neg) / 2) var(--n-blur) var(--n-shad-light);
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

.neuBox {
  width: 1.4em;
  height: 1.4em;
  background: var(--n-bg);
  border-radius: 30%; /* Forma suavizada neumórfica */
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  
  /* Estado inicial: Extruido (hacia afuera) */
  box-shadow: 
    var(--n-dist) var(--n-dist) var(--n-blur) var(--n-shad-dark),
    var(--n-dist-neg) var(--n-dist-neg) var(--n-blur) var(--n-shad-light);
}

/* Estado Marcado: Hundido (Inset) */
.checked .neuBox {
  box-shadow: 
    inset var(--n-dist) var(--n-dist) var(--n-blur) var(--n-shad-dark),
    inset var(--n-dist-neg) var(--n-dist-neg) var(--n-blur) var(--n-shad-light);
  transform: scale(0.96);
}

.checkIcon {
  color: var(--n-accent);
  width: 60%;
  height: 60%;
  filter: drop-shadow(0 0 1px rgba(0,0,0,0.1));
}

.label {
  font-size: 0.9rem;
  font-weight: 700;
  color: #6d7c90;
  letter-spacing: -0.01em;
}

/* Tamaños */
.sm { font-size: 14px; padding: 0.4rem 0.8rem; --n-dist: 2px; }
.md { font-size: 18px; }
.lg { font-size: 24px; padding: 0.8rem 1.4rem; --n-dist: 6px; }

.disabled {
  opacity: 0.5;
  cursor: not-allowed;
  filter: grayscale(1);
}

.neuWrapper:hover:not(.checked):not(.disabled) .neuBox {
  transform: translateY(-1px);
  box-shadow: 
    calc(var(--n-dist) * 1.5) calc(var(--n-dist) * 1.5) calc(var(--n-blur) * 1.5) var(--n-shad-dark),
    calc(var(--n-dist-neg) * 1.5) calc(var(--n-dist-neg) * 1.5) calc(var(--n-blur) * 1.5) var(--n-shad-light);
}

/* Animación de aparición del check */
:global(.neu-fade-enter-active) {
  transition: all 0.4s ease;
}
:global(.neu-fade-enter-from) {
  opacity: 0;
  transform: scale(0.5);
}
</style>