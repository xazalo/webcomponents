<template>
  <button
    v-bind="$attrs"
    :class="$style.speedButton"
    :style="buttonStyles"
    :disabled="disabled || loading"
  >
    <div :class="$style.trail"></div>
    
    <Icon v-if="loading" name="lucide:loader-2" :class="$style.iconSpin" />
    <Icon v-if="icon && !loading" :name="icon" :class="$style.buttonIcon" />

    <span v-if="$slots.default" :class="$style.buttonContent">
      <slot />
    </span>

    <div :class="$style.shimmer"></div>
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
  color?: string;
  textColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "#ff004c", // Rojo "Turbo"
  textColor: "#ffffff",
});

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.4rem 1.2rem", fontSize: "0.8rem", skew: "-12deg" },
    md: { padding: "0.6rem 2rem", fontSize: "0.95rem", skew: "-15deg" },
    lg: { padding: "1rem 3.5rem", fontSize: "1.1rem", skew: "-18deg" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-color": props.color,
    "--btn-text": props.textColor,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-skew": currentSize.skew,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
  };
});
</script>

<style module>
.speedButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-weight: 900;
  font-style: italic; /* Esencial para la sensación de velocidad */
  text-transform: uppercase;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: #000; /* Fondo oscuro base */
  color: var(--btn-text);
  cursor: pointer;
  border: none;
  outline: none;
  position: relative;
  overflow: hidden;
  
  /* Inclinación aerodinámica */
  transform: skewX(var(--btn-skew));
  border-left: 5px solid var(--btn-color);
}

.speedButton::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: var(--btn-color);
  z-index: 0;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

.buttonContent, .buttonIcon, .iconSpin {
  position: relative;
  z-index: 1;
  /* Corregimos la inclinación para el contenido */
  transform: skewX(calc(var(--btn-skew) * -1));
}

.speedButton:hover:not(:disabled) {
  transform: skewX(var(--btn-skew)) translateX(10px);
  box-shadow: -15px 0 30px -10px var(--btn-color);
}

.speedButton:hover::before {
  transform: scaleX(1);
}

.speedButton:hover .shimmer {
  animation: nitro 0.6s forwards;
}

.shimmer {
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.4),
    transparent
  );
  z-index: 2;
  transform: skewX(var(--btn-skew));
}

@keyframes nitro {
  100% { left: 150%; }
}

.speedButton:active:not(:disabled) {
  transform: skewX(var(--btn-skew)) translateX(5px) scale(0.95);
}

.speedButton:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  transform: skewX(var(--btn-skew));
}

.iconSpin {
  animation: spin 0.5s linear infinite; /* Rotación más rápida */
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>