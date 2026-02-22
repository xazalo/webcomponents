<template>
  <button
    v-bind="$attrs"
    :class="$style.neuButton"
    :style="buttonStyles"
    :disabled="disabled || loading"
  >
    <Icon v-if="loading" name="lucide:loader-2" :class="$style.iconSpin" />

    <Icon v-if="icon && !loading" :name="icon" :class="$style.buttonIcon" />

    <span v-if="$slots.default" :class="$style.buttonContent">
      <slot />
    </span>
  </button>
</template>

<script setup lang="ts">
import { computed } from "vue";

defineOptions({ inheritAttrs: false });

interface Props {
  variant?: "primary" | "secondary" | "outline";
  size?: "sm" | "md" | "lg";
  block?: boolean;
  disabled?: boolean;
  loading?: boolean;
  icon?: string;
  color?: string; // Color base del sistema (ej: #e0e5ec)
  textColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "#e0e5ec", // Gris azulado clásico de Neumorfismo
  textColor: "#6d7c90",
});

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.5rem 1rem", fontSize: "0.85rem", radius: "12px", offset: "4px" },
    md: { padding: "0.8rem 1.8rem", fontSize: "1rem", radius: "16px", offset: "6px" },
    lg: { padding: "1.2rem 3rem", fontSize: "1.15rem", radius: "24px", offset: "10px" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-bg": props.color,
    "--btn-text": props.textColor,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-radius": currentSize.radius,
    "--btn-offset": currentSize.offset,
    "--btn-offset-neg": `-${currentSize.offset}`,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
  };
});
</script>

<style module>
.neuButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  font-weight: 700;
  transition: all 0.2s ease;
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: var(--btn-bg);
  color: var(--btn-text);
  cursor: pointer;
  border-radius: var(--btn-radius);
  border: none;
  outline: none;
  position: relative;
  user-select: none;

  /* NEUMORPHISM: Relieve hacia afuera */
  box-shadow: 
    var(--btn-offset) var(--btn-offset) calc(var(--btn-offset) * 2) rgba(163, 177, 198, 0.6), 
    var(--btn-offset-neg) var(--btn-offset-neg) calc(var(--btn-offset) * 2) rgba(255, 255, 255, 0.8);
}

.neuButton:hover:not(:disabled) {
  /* Al hover, se "eleva" un poco más */
  box-shadow: 
    calc(var(--btn-offset) + 2px) calc(var(--btn-offset) + 2px) calc(var(--btn-offset) * 3) rgba(163, 177, 198, 0.7), 
    calc(var(--btn-offset-neg) - 2px) calc(var(--btn-offset-neg) - 2px) calc(var(--btn-offset) * 3) rgba(255, 255, 255, 1);
  transform: scale(1.01);
}

.neuButton:active:not(:disabled) {
  /* Efecto de pulsado: de relieve a hundido (inset) */
  box-shadow: 
    inset 4px 4px 8px rgba(163, 177, 198, 0.6), 
    inset -4px -4px 8px rgba(255, 255, 255, 0.8);
  transform: scale(0.98);
}

.neuButton:disabled {
  cursor: not-allowed;
  opacity: 0.5;
  box-shadow: none; /* Los elementos deshabilitados suelen ser planos en este estilo */
  border: 1px solid rgba(0,0,0,0.05);
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>