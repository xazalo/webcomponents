<template>
  <button
    v-bind="$attrs"
    :class="$style.bentoButton"
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
  hoverColor?: string;
  textColor?: string;
  rounded?: string;
  shadow?: string | boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  block: false,
  disabled: false,
  loading: false,
  icon: "",
  color: "#4f46e5",
  hoverColor: "#4338ca",
  textColor: "#ffffff",
  rounded: "1rem", // Radio más amplio para estilo bento
  shadow: true,
});

const DEFAULT_SHADOW = "0 4px 12px rgba(0, 0, 0, 0.08), 0 2px 4px rgba(0, 0, 0, 0.04)";

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.4rem 1rem", fontSize: "0.85rem" },
    md: { padding: "0.6rem 1.5rem", fontSize: "0.95rem" },
    lg: { padding: "0.9rem 2.5rem", fontSize: "1.1rem" },
  };

  const currentSize = sizeMap[props.size];

  return {
    "--btn-bg": props.variant === "outline" ? "white" : props.color,
    "--btn-hover-bg": props.variant === "outline" ? "#f9fafb" : props.hoverColor,
    "--btn-text": props.variant === "outline" ? props.color : props.textColor,
    "--btn-radius": props.rounded,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
    "--btn-shadow": props.shadow ? (typeof props.shadow === "string" ? props.shadow : DEFAULT_SHADOW) : "none",
    "--btn-border": props.variant === "outline" ? `1px solid rgba(0,0,0,0.1)` : "none",
  };
});
</script>

<style module>
.bentoButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  font-weight: 600;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: var(--btn-bg);
  color: var(--btn-text);
  cursor: pointer;
  border-radius: var(--btn-radius);
  border: var(--btn-border);
  box-shadow: var(--btn-shadow);
  outline: none;
  position: relative;
  overflow: hidden;
}

/* El toque Bento: Un borde interno muy sutil para dar relieve */
.bentoButton::after {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: var(--btn-radius);
  border: 1px solid rgba(255, 255, 255, 0.15);
  pointer-events: none;
}

.bentoButton:hover:not(:disabled) {
  background-color: var(--btn-hover-bg);
  transform: translateY(-2px) scale(1.02);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.bentoButton:active:not(:disabled) {
  transform: translateY(0) scale(0.98);
}

.bentoButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  filter: grayscale(0.5);
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>