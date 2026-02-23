<template>
  <div 
    v-bind="$attrs" 
    :class="[$style.brutalRoot, $style[size]]" 
    :style="brutalStyles"
  >
    <label v-if="label" :class="$style.label">{{ label }}</label>
    
    <div :class="$style.inputWrapper">
      <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :disabled="disabled"
        :class="[
          $style.input, 
          { [$style.hasIcon]: $slots.icon }
        ]"
      />
      
      <div v-if="$slots.icon" :class="$style.iconArea">
        <slot name="icon" />
      </div>

      <div v-if="modelValue && showStatus" :class="$style.statusBadge">
        OK
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
  disabled?: boolean;
  // Personalización Brutal
  accentColor?: string;   /* Color al enfocar */
  shadowColor?: string;   /* Color de la sombra sólida */
  borderWidth?: string;   /* Grosor del trazo */
  shadowSize?: string;    /* Distancia de la sombra */
  size?: 'sm' | 'md' | 'lg';
  showStatus?: boolean;   /* Muestra un pequeño badge al escribir */
}

const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'ESCRIBE AQUÍ...',
  accentColor: '#bc95ff',
  shadowColor: '#000000',
  borderWidth: '3px',
  shadowSize: '6px',
  size: 'md',
  showStatus: false
});

defineEmits(['update:modelValue']);

const brutalStyles = computed(() => ({
  '--brutal-accent': props.accentColor,
  '--brutal-shadow': props.shadowColor,
  '--brutal-bw': props.borderWidth,
  '--brutal-sw': props.shadowSize,
  '--brutal-sw-hover': `calc(${props.shadowSize} + 2px)`,
}));
</script>

<style module>
.brutalRoot {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  width: 100%;
}

.label {
  font-size: 0.9rem;
  font-weight: 900;
  text-transform: uppercase;
  color: var(--brutal-shadow);
  letter-spacing: 0.05em;
}

.inputWrapper {
  position: relative;
  width: 100%;
}

.input {
  width: 100%;
  background: #ffffff;
  border: var(--brutal-bw) solid var(--brutal-shadow);
  border-radius: 4px;
  color: #000000;
  font-weight: 800;
  outline: none;
  box-shadow: var(--brutal-sw) var(--brutal-sw) 0px 0px var(--brutal-shadow);
  transition: all 0.1s cubic-bezier(0.17, 0.67, 0.83, 0.67);
  appearance: none;
}

/* Tamaños */
.sm .input { padding: 0.6rem 0.8rem; font-size: 0.9rem; }
.md .input { padding: 1rem 1.2rem; font-size: 1.1rem; }
.lg .input { padding: 1.4rem 1.6rem; font-size: 1.4rem; }

.input::placeholder {
  color: var(--brutal-shadow);
  opacity: 0.4;
  text-transform: uppercase;
}

/* Estado Focus: Salto visual */
.input:focus {
  background: var(--brutal-accent);
  transform: translate(-2px, -2px);
  box-shadow: var(--brutal-sw-hover) var(--brutal-sw-hover) 0px 0px var(--brutal-shadow);
}

/* Iconos en Neo-Brutalismo: Siempre con trazos fuertes */
.hasIcon { padding-left: 3.5rem !important; }

.iconArea {
  position: absolute;
  left: 1.2rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 2;
  display: flex;
  align-items: center;
  font-size: 1.2em;
  color: var(--brutal-shadow);
  pointer-events: none;
}

/* Badge de estado tipo "sticker" */
.statusBadge {
  position: absolute;
  right: -10px;
  top: -10px;
  background: #00ff41; /* Verde Matrix */
  border: 2px solid #000;
  padding: 2px 6px;
  font-size: 0.7rem;
  font-weight: 900;
  transform: rotate(15deg);
  z-index: 3;
}

.input:active {
  transform: translate(var(--brutal-sw), var(--brutal-sw));
  box-shadow: 0px 0px 0px 0px var(--brutal-shadow);
}

.input:disabled {
  background: #e5e7eb;
  cursor: not-allowed;
  box-shadow: none;
  transform: none;
}
</style>