<template>
  <label 
    v-bind="$attrs" 
    :class="[
      $style.clayWrapper, 
      $style[size], 
      { [$style.checked]: modelValue }
    ]" 
    :style="clayStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.clayBox">
        <Transition name="clay-pop">
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
  // Colores
  accentColor?: string;   /* Color de la arcilla cuando se activa */
  baseColor?: string;     /* Fondo del componente (wrapper) */
  textColor?: string;     /* Color de la etiqueta */
  // Estética
  size?: 'sm' | 'md' | 'lg';
  borderRadius?: string;
  depth?: number;         /* Intensidad del relieve (0 a 1) */
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  accentColor: '#f472b6',
  baseColor: '#f0f4f8',
  textColor: '#64748b',
  size: 'md',
  borderRadius: '25px',
  depth: 0.5
});

defineEmits(['update:modelValue']);

const clayStyles = computed(() => ({
  '--c-accent': props.accentColor,
  '--c-bg': props.baseColor,
  '--c-text': props.textColor,
  '--c-radius': props.borderRadius,
  // Sombras dinámicas basadas en profundidad
  '--c-inner-light': `inset 0 ${4 * props.depth}px ${6 * props.depth}px rgba(255, 255, 255, 0.8)`,
  '--c-inner-dark': `inset 0 -${6 * props.depth}px ${8 * props.depth}px rgba(0, 0, 0, ${0.1 * props.depth})`,
  '--c-outer-shadow': `0 ${10 * props.depth}px ${20 * props.depth}px -5px rgba(0, 0, 0, 0.08)`,
}));
</script>

<style module>
.clayWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.8rem;
  cursor: pointer;
  padding: 0.6rem 1.2rem;
  background: var(--c-bg);
  border-radius: var(--c-radius);
  user-select: none;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: var(--c-outer-shadow), var(--c-inner-light);
}

.clayWrapper:hover {
  transform: translateY(-3px);
  filter: brightness(1.02);
  box-shadow: 0 15px 25px -5px rgba(0, 0, 0, 0.1), var(--c-inner-light);
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

.clayBox {
  width: 1.5em;
  height: 1.5em;
  background: white;
  border-radius: calc(var(--c-radius) * 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: 
    inset 0 2px 4px rgba(255, 255, 255, 1),
    inset 0 -3px 6px rgba(0, 0, 0, 0.05),
    0 4px 10px rgba(0, 0, 0, 0.05);
}

/* Estado Marcado */
.checked .clayBox {
  background: var(--c-accent);
  transform: scale(1.15) rotate(3deg);
  box-shadow: 
    var(--c-inner-light),
    var(--c-inner-dark),
    0 8px 15px rgba(0, 0, 0, 0.1);
}

.checkIcon {
  color: white;
  width: 65%;
  height: 65%;
  filter: drop-shadow(0 2px 3px rgba(0, 0, 0, 0.1));
}

.label {
  font-weight: 700;
  color: var(--c-text);
  letter-spacing: -0.01em;
}

/* Tamaños */
.sm { font-size: 0.8rem; padding: 0.4rem 0.9rem; }
.md { font-size: 1rem; }
.lg { font-size: 1.2rem; padding: 0.8rem 1.6rem; }

/* Animación de entrada */
:global(.clay-pop-enter-active) {
  animation: clay-pop 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
@keyframes clay-pop {
  0% { transform: scale(0) rotate(-10deg); opacity: 0; }
  100% { transform: scale(1) rotate(0); opacity: 1; }
}

.clayWrapper:active {
  transform: scale(0.92) translateY(2px);
}
</style>