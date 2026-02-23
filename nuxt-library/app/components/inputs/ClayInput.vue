<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.clayRoot, $style[size]]" 
    :style="clayStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.inputWrapper">
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="[$style.input, { [$style.hasIcon]: $slots.icon }]"
      />
      
      <div v-if="$slots.icon" :class="$style.iconArea">
        <slot name="icon" />
      </div>

      <div :class="$style.surfaceGlow" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: string | number;
  label?: string;
  placeholder?: string;
  type?: string;
  disabled?: boolean;
  // Personalización Clay
  accentColor?: string;   /* Color del texto/icono al enfocar */
  surfaceColor?: string;  /* Color base de la "arcilla" */
  borderRadius?: string;  /* Curvatura de la pieza */
  depth?: number;         /* Intensidad del relieve (0.1 a 1) */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Talla algo aquí...',
  accentColor: '#38bdf8',
  surfaceColor: '#f0f4f8',
  borderRadius: '20px',
  depth: 0.6,
  size: 'md'
});

defineEmits(['update:modelValue']);

const clayStyles = computed(() => ({
  '--clay-accent': props.accentColor,
  '--clay-bg': props.surfaceColor,
  '--clay-radius': props.borderRadius,
  // Cálculo de sombras dinámicas según profundidad
  '--clay-shadow-ext': `0 ${10 * props.depth}px ${20 * props.depth}px -5px rgba(0, 0, 0, 0.06)`,
  '--clay-shadow-int-top': `inset 0 ${4 * props.depth}px ${6 * props.depth}px rgba(255, 255, 255, 0.9)`,
  '--clay-shadow-int-bot': `inset 0 -${6 * props.depth}px ${10 * props.depth}px rgba(0, 0, 0, 0.05)`,
}));
</script>

<style module>
.clayRoot {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  width: 100%;
}

.label {
  font-size: 0.85rem;
  font-weight: 800;
  color: #64748b;
  padding-left: 0.75rem;
  letter-spacing: 0.02em;
}

.inputWrapper {
  position: relative;
  width: 100%;
}

.input {
  width: 100%;
  background: var(--clay-bg);
  border: none;
  border-radius: var(--clay-radius);
  color: #475569;
  font-weight: 700;
  outline: none;
  /* El corazón del Claymorphism */
  box-shadow: 
    var(--clay-shadow-ext),
    var(--clay-shadow-int-top),
    var(--clay-shadow-int-bot);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* Tamaños */
.sm .input { padding: 0.7rem 1.2rem; font-size: 0.9rem; }
.md .input { padding: 1rem 1.5rem; font-size: 1rem; }
.lg .input { padding: 1.3rem 1.8rem; font-size: 1.2rem; }

.input::placeholder {
  color: #94a3b8;
  font-weight: 500;
}

/* Focus: Inflado y suavizado */
.input:focus {
  background: #ffffff;
  transform: scale(1.02) translateY(-3px);
  color: var(--clay-accent);
  box-shadow: 
    0 20px 30px -10px rgba(0, 0, 0, 0.08),
    inset 0 4px 10px rgba(255, 255, 255, 1),
    inset 0 -2px 5px rgba(0, 0, 0, 0.02);
}

/* Iconos */
.hasIcon { padding-left: 3.5rem !important; }

.iconArea {
  position: absolute;
  left: 1.25rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 2;
  color: #94a3b8;
  pointer-events: none;
  transition: color 0.3s;
}

.input:focus ~ .iconArea {
  color: var(--clay-accent);
}

/* Brillo de superficie (Glow) */
.surfaceGlow {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: var(--clay-radius);
  pointer-events: none;
  box-shadow: inset 0 1px 1px rgba(255, 255, 255, 0.5);
  opacity: 0.5;
}

.input:disabled {
  opacity: 0.6;
  background: #e2e8f0;
  box-shadow: none;
  cursor: not-allowed;
}
</style>