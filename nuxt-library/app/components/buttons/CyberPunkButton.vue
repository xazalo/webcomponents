<template>
  <button
    v-bind="$attrs"
    :class="$style.customButton"
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

defineOptions({
  inheritAttrs: false
});

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
  borderColor?: string;
  borderSize?: string;
  borderStyle?: string;
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
  rounded: "0.5rem",
  borderSize: "2px",
  borderStyle: "solid",
  shadow: true,
});

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)";

const buttonStyles = computed(() => {
  const sizeMap = {
    sm: { padding: "0.25rem 0.75rem", fontSize: "0.875rem", path: "8px" },
    md: { padding: "0.5rem 1.25rem", fontSize: "1rem", path: "12px" },
    lg: { padding: "0.75rem 2.25rem", fontSize: "1.125rem", path: "16px" },
  };

  const currentSize = sizeMap[props.size];

  let finalShadow = "none";
  if (typeof props.shadow === "string") {
    finalShadow = props.shadow;
  } else if (props.shadow === true) {
    finalShadow = DEFAULT_SHADOW;
  }

  return {
    "--btn-color": props.color,
    "--btn-hover": props.hoverColor,
    "--btn-text": props.textColor,
    "--btn-radius": props.rounded,
    "--btn-border-color": props.borderColor || props.color,
    "--btn-border-size": props.borderSize,
    "--btn-border-style": props.borderStyle,
    "--btn-padding": currentSize.padding,
    "--btn-font-size": currentSize.fontSize,
    "--btn-path": currentSize.path,
    "--btn-bg": props.variant === "outline" ? "transparent" : props.color,
    "--btn-color-final": props.variant === "outline" ? props.color : props.textColor,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
    "--btn-opacity": props.disabled || props.loading ? "0.5" : "1",
    "--btn-pointer": props.disabled || props.loading ? "not-allowed" : "pointer",
    "--btn-shadow": finalShadow,
  };
});
</script>

<style module>
.customButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-weight: 600;
  transition: all 0.2s ease;
  padding: var(--btn-padding);
  font-size: var(--btn-font-size);
  background-color: var(--btn-bg);
  color: var(--btn-color-final);
  cursor: var(--btn-pointer);
  opacity: var(--btn-opacity);
  border: var(--btn-border-size) var(--btn-border-style) var(--btn-border-color);
  outline: none;
  box-sizing: border-box;
  box-shadow: var(--btn-shadow);
  clip-path: polygon(
    var(--btn-path) 0%,
    100% 0%,
    100% calc(100% - var(--btn-path)),
    calc(100% - var(--btn-path)) 100%,
    0% 100%,
    0% var(--btn-path)
  );
}

.customButton:hover:not(:disabled) {
  background-color: var(--btn-hover);
  border-color: var(--btn-hover);
  color: var(--btn-text);
  transform: translateY(-1px);
}

.customButton:active:not(:disabled) {
  transform: translateY(0);
}

.buttonContent,
.buttonIcon {
  display: flex;
  align-items: center;
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>