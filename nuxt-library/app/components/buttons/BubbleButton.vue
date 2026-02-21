<template>
  <button
    v-bind="$attrs"
    :class="$style.bubbleButton"
    :style="buttonStyles"
    :disabled="disabled || loading"
  >
    <Icon v-if="loading" name="lucide:loader-2" :class="$style.iconSpin" />
    <Icon v-if="icon && !loading" :name="icon" />
    <span v-if="$slots.default">
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
  variant?: "primary" | "outline";
  block?: boolean;
  loading?: boolean;
  icon?: string;
  color?: string;
  hoverColor?: string;
  textColor?: string;
  blobbiness?: number;
  shadow?: string | boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  block: false,
  loading: false,
  icon: "",
  color: "#4f46e5",
  hoverColor: "#4338ca",
  textColor: "#ffffff",
  blobbiness: 1,
  shadow: true,
});

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)";

const buttonStyles = computed(() => {
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
    "--radius-1": `${30 * props.blobbiness}%`,
    "--radius-2": `${70 * props.blobbiness}%`,
    "--btn-display": props.block ? "flex" : "inline-flex",
    "--btn-width": props.block ? "100%" : "auto",
    "--btn-bg": props.variant === "outline" ? "transparent" : props.color,
    "--btn-color-final": props.variant === "outline" ? props.color : props.textColor,
    "--btn-shadow": finalShadow,
  };
});
</script>

<style module>
.bubbleButton {
  display: var(--btn-display);
  width: var(--btn-width);
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  padding: 0.8rem 1.6rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  border: 2px solid var(--btn-color);
  outline: none;
  box-sizing: border-box;
  background-color: var(--btn-bg);
  color: var(--btn-color-final);
  box-shadow: var(--btn-shadow);
  border-radius: 60% 40% 70% 30% / 40% 50% 60% 40%;
}

.bubbleButton:hover:not(:disabled) {
  background-color: var(--btn-hover);
  border-color: var(--btn-hover);
  color: white;
  border-radius: 40% 60% 30% 70% / 60% 40% 70% 30%;
  transform: scale(1.05) translateY(-2px);
}

.bubbleButton:active:not(:disabled) {
  transform: scale(0.95);
}

.bubbleButton:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.iconSpin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>