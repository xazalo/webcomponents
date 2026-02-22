<template>
  <div v-bind="$attrs" :class="$style.bentoRoot" :style="bentoStyles">
    <div :class="$style.inputWrapper">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        :placeholder="placeholder"
        :class="$style.input"
      />
      <div :class="$style.innerGlow"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  modelValue: string | number;
  placeholder?: string;
  color?: string;
}>();

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoRoot {
  width: 100%;
}

.inputWrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.input {
  width: 100%;
  padding: 0.9rem 1.2rem;
  font-size: 0.95rem;
  font-weight: 500;
  color: #1f2937;
  background: #ffffff;
  /* Radio Bento: consistente y suave */
  border-radius: 0.8rem;
  /* Borde muy fino casi imperceptible */
  border: 1px solid rgba(0, 0, 0, 0.08);
  outline: none;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 
    0 1px 2px rgba(0, 0, 0, 0.05),
    0 4px 6px -1px rgba(0, 0, 0, 0.02);
}

.input::placeholder {
  color: #9ca3af;
  transition: opacity 0.2s;
}

/* Estado Focus: Elevación y acento */
.input:focus {
  background: #ffffff;
  border-color: var(--b-accent);
  transform: translateY(-1px);
  /* Sombra más profunda para simular elevación de la "celda" */
  box-shadow: 
    0 10px 15px -3px rgba(0, 0, 0, 0.05),
    0 4px 6px -2px rgba(0, 0, 0, 0.03);
}

.input:focus::placeholder {
  opacity: 0.5;
}

/* El toque premium Bento: un brillo sutil en la parte superior */
.innerGlow {
  position: absolute;
  inset: 0;
  border-radius: 0.8rem;
  pointer-events: none;
  box-shadow: inset 0 1px 1px rgba(255, 255, 255, 0.8);
  opacity: 1;
}

.input:focus ~ .innerGlow {
  box-shadow: inset 0 1px 1px rgba(var(--b-accent), 0.1);
}
</style>