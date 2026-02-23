<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.neuRoot, $style[size], { [$style.disabled]: disabled }]" 
    :style="neuStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.inputWrapper">
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="[$style.input, { [$style.flat]: appearance === 'flat' }]"
      />
      
      <div v-if="icon" :class="$style.iconArea">
        <Icon :name="icon" />
      </div>
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
  icon?: string;
  disabled?: boolean;
  // Personalización Neumórfica
  baseColor?: string;     /* El color exacto del fondo de tu web */
  accentColor?: string;   /* Color del texto al enfocar */
  intensity?: number;     /* Opacidad de las sombras (0.1 a 1) */
  distance?: number;      /* Profundidad del hundimiento en px */
  size?: 'sm' | 'md' | 'lg';
  appearance?: 'inset' | 'flat'; /* 'inset' es el estándar para inputs */
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Escribe en la superficie...',
  baseColor: '#e0e5ec',
  accentColor: '#4f46e5',
  intensity: 0.5,
  distance: 6,
  size: 'md',
  appearance: 'inset'
});

defineEmits(['update:modelValue']);

const neuStyles = computed(() => {
  const dist = props.distance;
  const blur = dist * 2;
  
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.accentColor,
    '--n-radius': props.size === 'sm' ? '8px' : '16px',
    // Sombras calculadas
    '--n-shad-dark': `rgba(163, 177, 198, ${props.intensity})`,
    '--n-shad-light': `rgba(255, 255, 255, ${props.intensity + 0.2})`,
    '--n-dist': `${dist}px`,
    '--n-dist-neg': `-${dist}px`,
    '--n-blur': `${blur}px`,
  };
});
</script>

<style module>
.neuRoot {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  width: 100%;
}

.label {
  font-size: 0.85rem;
  font-weight: 700;
  color: #6d7c90;
  padding-left: 0.5rem;
  letter-spacing: 0.02em;
}

.inputWrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.input {
  width: 100%;
  background: var(--n-bg);
  border: none;
  border-radius: var(--n-radius);
  color: #444f5a;
  font-weight: 600;
  outline: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  
  /* Efecto hundido (Cavidad en la superficie) */
  box-shadow: 
    inset var(--n-dist) var(--n-dist) var(--n-blur) var(--n-shad-dark),
    inset var(--n-dist-neg) var(--n-dist-neg) var(--n-blur) var(--n-shad-light);
}

/* Estado Focus: El hundimiento se vuelve más nítido o "profundo" */
.input:focus {
  color: var(--n-accent);
  box-shadow: 
    inset calc(var(--n-dist) * 1.5) calc(var(--n-dist) * 1.5) calc(var(--n-blur) * 1.2) var(--n-shad-dark),
    inset calc(var(--n-dist-neg) * 1.5) calc(var(--n-dist-neg) * 1.5) calc(var(--n-blur) * 1.2) var(--n-shad-light);
}

.input::placeholder {
  color: #9caab9;
  font-weight: 400;
}

/* Tamaños */
.sm .input { padding: 0.7rem 1rem; font-size: 0.85rem; }
.md .input { padding: 1rem 1.4rem; font-size: 1rem; }
.lg .input { padding: 1.3rem 1.8rem; font-size: 1.15rem; }

/* Soporte para iconos */
.iconArea {
  position: absolute;
  right: 1.2rem;
  color: #9caab9;
  pointer-events: none;
  transition: color 0.3s;
}

.input:focus ~ .iconArea {
  color: var(--n-accent);
}

.disabled {
  opacity: 0.5;
  filter: grayscale(1);
  cursor: not-allowed;
}

/* Variante plana (Solo sombra suave, sin hundimiento) */
.flat {
  box-shadow: 2px 2px 5px var(--n-shad-dark), -2px -2px 5px var(--n-shad-light);
}
</style>