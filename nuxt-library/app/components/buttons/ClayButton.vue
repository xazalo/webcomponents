<template>
  <button
    v-bind="$attrs"
    :class="$style.clayButton"
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
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "#84cc16", // Verde lima suave tipo "slime"
  textColor: "#ffffff",
});

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.5rem 1rem", fontSize: "0.85rem", radius: "12px" },
    md: { padding: "0.8rem 1.8rem", fontSize: "1rem", radius: "20px" },
    lg: { padding: "1.2rem 3rem", fontSize: "1.15rem", radius: "30px" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-bg": props.variant === "outline" ? "#f8fafc" : props.color,
    "--btn-text": props.variant === "outline" ? props.color : props.textColor,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-radius": currentSize.radius,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
  };
});
</script>

<style module>
.clayButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  font-weight: 800;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
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

  /* EL EFECTO CLAY: Sombra externa + Brillo interno superior + Sombra interna inferior */
  box-shadow: 
    0 10px 20px -5px rgba(0, 0, 0, 0.1),
    inset 0 4px 8px rgba(255, 255, 255, 0.7),
    inset 0 -6px 12px rgba(0, 0, 0, 0.1);
}

.clayButton:hover:not(:disabled) {
  transform: translateY(-4px) scale(1.03);
  box-shadow: 
    0 15px 25px -5px rgba(0, 0, 0, 0.15),
    inset 0 6px 10px rgba(255, 255, 255, 0.8),
    inset 0 -8px 15px rgba(0, 0, 0, 0.1);
}

.clayButton:active:not(:disabled) {
  transform: translateY(2px) scale(0.97);
  box-shadow: 
    0 5px 10px -2px rgba(0, 0, 0, 0.1),
    inset 0 2px 4px rgba(255, 255, 255, 0.5),
    inset 0 -3px 6px rgba(0, 0, 0, 0.1);
}

.clayButton:disabled {
  cursor: not-allowed;
  filter: grayscale(0.8);
  opacity: 0.7;
}

.buttonContent {
  /* Un ligero sombreado al texto lo integra mejor en el botón "inflado" */
  text-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>