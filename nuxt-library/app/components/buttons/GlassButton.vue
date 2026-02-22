<template>
  <button
    v-bind="$attrs"
    :class="$style.glassButton"
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
  color?: string;    /* Color del tinte del cristal */
  textColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "rgba(255, 255, 255, 0.2)", // Blanco traslúcido por defecto
  textColor: "#ffffff",
});

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.5rem 1rem", fontSize: "0.85rem", radius: "12px" },
    md: { padding: "0.8rem 1.8rem", fontSize: "1rem", radius: "16px" },
    lg: { padding: "1.2rem 3rem", fontSize: "1.15rem", radius: "24px" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-bg": props.color,
    "--btn-text": props.textColor,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-radius": currentSize.radius,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
  };
});
</script>

<style module>
.glassButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  font-weight: 600;
  transition: all 0.3s ease;
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: var(--btn-bg);
  color: var(--btn-text);
  cursor: pointer;
  border-radius: var(--btn-radius);
  outline: none;
  position: relative;
  overflow: hidden;

  /* EFECTO GLASSMORPHISM */
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  
  /* Borde sutil que simula el grosor del cristal */
  border: 1px solid rgba(255, 255, 255, 0.3);
  
  /* Sombra externa muy suave y brillo interno */
  box-shadow: 
    0 8px 32px 0 rgba(0, 0, 0, 0.1),
    inset 0 0 0 1px rgba(255, 255, 255, 0.1);
}



.glassButton:hover:not(:disabled) {
  background-color: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
  border-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 12px 40px 0 rgba(0, 0, 0, 0.2);
}

.glassButton:active:not(:disabled) {
  transform: translateY(1px);
  background-color: rgba(255, 255, 255, 0.1);
}

.glassButton:disabled {
  cursor: not-allowed;
  opacity: 0.4;
  backdrop-filter: blur(4px);
}

.buttonContent {
  /* Mejora la legibilidad sobre fondos complejos */
  text-shadow: 0 1px 2px rgba(0,0,0,0.2);
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>