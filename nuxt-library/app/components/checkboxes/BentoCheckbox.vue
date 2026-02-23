<template>
  <label 
    v-bind="$attrs" 
    :class="[$style.bentoWrapper, $style[size], { [$style.checked]: modelValue }]" 
    :style="checkboxStyles"
  >
    <div :class="$style.inputContainer">
      <input 
        type="checkbox" 
        :checked="modelValue" 
        @change="$emit('update:modelValue', $event.target.checked)"
      />
      <div :class="$style.bentoBox">
        <Transition name="pop">
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
  // Colores personalizables
  accentColor?: string;   /* Color cuando está marcado */
  baseColor?: string;     /* Color de fondo del wrapper */
  boxColor?: string;      /* Color del checkbox desmarcado */
  textColor?: string;     /* Color de la etiqueta */
  // Estética Bento
  size?: 'sm' | 'md' | 'lg';
  borderRadius?: string;  /* Radio de las esquinas */
  borderWidth?: string;   /* Grosor del borde */
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  accentColor: '#4f46e5',
  baseColor: '#ffffff',
  boxColor: '#f3f4f6',
  textColor: '#374151',
  size: 'md',
  borderRadius: '0.75rem',
  borderWidth: '1px'
});

defineEmits(['update:modelValue']);

const checkboxStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-bg': props.baseColor,
  '--b-box': props.boxColor,
  '--b-text': props.textColor,
  '--b-radius': props.borderRadius,
  '--b-radius-inner': `calc(${props.borderRadius} * 0.5)`,
  '--b-bw': props.borderWidth,
}));
</script>

<style module>
.bentoWrapper {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  padding: 0.5rem 0.8rem;
  background: var(--b-bg);
  border-radius: var(--b-radius);
  border: var(--b-bw) solid rgba(0, 0, 0, 0.08);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  user-select: none;
}

.bentoWrapper:hover {
  transform: translateY(-1px);
  filter: brightness(0.98);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
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

.bentoBox {
  width: 1.4em;
  height: 1.4em;
  background: var(--b-box);
  border-radius: var(--b-radius-inner);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
  border: var(--b-bw) solid rgba(0, 0, 0, 0.1);
}

/* Estado Marcado */
.checked .bentoBox {
  background: var(--b-accent);
  border-color: var(--b-accent);
  box-shadow: 0 4px 12px v-bind('accentColor + "4D"'); /* Sombra con 30% alpha */
}

.checkIcon {
  color: white;
  width: 70%;
  height: 70%;
}

.label {
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--b-text);
  letter-spacing: -0.01em;
}

/* Tamaños */
.sm { font-size: 12px; padding: 0.4rem 0.6rem; }
.md { font-size: 16px; padding: 0.5rem 0.9rem; }
.lg { font-size: 20px; padding: 0.7rem 1.2rem; }

/* Animación de entrada del check */
:global(.pop-enter-active) {
  animation: check-pop 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}
@keyframes check-pop {
  0% { transform: scale(0); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

.bentoWrapper:active .bentoBox {
  transform: scale(0.85);
}
</style>