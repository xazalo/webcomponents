<template>
  <button
    v-bind="$attrs"
    :class="$style.brutalButton"
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
  color?: string;
  textColor?: string;
  shadowColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "#FFDE00", // Amarillo brutalista por defecto
  textColor: "#000000",
  shadowColor: "#000000",
});

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.4rem 0.8rem", fontSize: "0.85rem", shadow: "3px" },
    md: { padding: "0.6rem 1.4rem", fontSize: "1rem", shadow: "5px" },
    lg: { padding: "1rem 2.5rem", fontSize: "1.2rem", shadow: "8px" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-bg": props.variant === "outline" ? "#ffffff" : props.color,
    "--btn-text": props.textColor,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-shadow-size": currentSize.shadow,
    "--btn-shadow-color": props.shadowColor,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
  };
});
</script>

<style module>
.brutalButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  font-weight: 900; /* Extra bold para el estilo */
  text-transform: uppercase;
  letter-spacing: 0.05em;
  transition: all 0.1s ease-out;
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: var(--btn-bg);
  color: var(--btn-text);
  cursor: pointer;
  
  /* Bordes y Sombras Brutalistas */
  border: 3px solid #000000;
  border-radius: 0px; /* Cuadrado puro o muy poco radio */
  box-shadow: var(--btn-shadow-size) var(--btn-shadow-size) 0px 0px var(--btn-shadow-color);
  
  outline: none;
  position: relative;
  user-select: none;
}

.brutalButton:hover:not(:disabled) {
  /* Al hover, el botón se desplaza pero la sombra crece */
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--btn-shadow-size) + 2px) calc(var(--btn-shadow-size) + 2px) 0px 0px var(--btn-shadow-color);
}

.brutalButton:active:not(:disabled) {
  /* Efecto de pulsado: el botón se mueve sobre su propia sombra */
  transform: translate(var(--btn-shadow-size), var(--btn-shadow-size));
  box-shadow: 0px 0px 0px 0px var(--btn-shadow-color);
}

.brutalButton:disabled {
  cursor: not-allowed;
  background-color: #d1d5db;
  color: #6b7280;
  box-shadow: 2px 2px 0px 0px #6b7280;
}

.buttonIcon {
  font-size: 1.3em;
  stroke-width: 3px; /* Iconos más gruesos */
}

.iconSpin {
  animation: spin 0.8s steps(8) infinite; /* Animación más "tosca" */
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>