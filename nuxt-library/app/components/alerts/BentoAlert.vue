<template>
  <div :class="[$style.bentoAlert, $style[type]]" :style="dynamicStyles">
    <div :class="[$style.cell, $style.mainCell]">
      <div :class="$style.iconWrapper">
        <slot name="icon">
          <span :class="$style.defaultIcon">!</span>
        </slot>
      </div>
      <div :class="$style.headerText">
        <span v-if="label" :class="$style.label">{{ label }}</span>
        <h3 :class="$style.title">{{ title }}</h3>
      </div>
    </div>

    <div :class="[$style.cell, $style.contentCell]">
      <p :class="$style.description">{{ message }}</p>
    </div>

    <div :class="[$style.cell, $style.actionCell]" @click="$emit('action')">
      <span :class="$style.actionLabel">{{ actionText }}</span>
      <div :class="$style.arrow">→</div>
    </div>

    <button :class="[$style.cell, $style.closeCell]" @click="$emit('close')">
      ✕
    </button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title: string;
  message: string;
  type?: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  label?: string;
  // Dynamic color props
  accentColor?: string;
  backgroundColor?: string;
  borderColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  type: 'info',
  accentColor: '#A2D2FF', // Default fallback
  backgroundColor: '#ffffff',
  borderColor: '#000000'
});

defineEmits(['close', 'action']);

// Map the "type" to default colors if accentColor isn't explicitly changed,
// or just use the prop directly for full control.
const dynamicStyles = computed(() => {
  const typeColors = {
    info: '#A2D2FF',
    warning: '#FFD6A5',
    error: '#FFADAD',
    success: '#CAFFBF'
  };

  return {
    '--current-accent': props.accentColor || typeColors[props.type],
    '--bento-bg': props.backgroundColor,
    '--bento-border': props.borderColor,
  };
});
</script>

<style module>
.bentoAlert {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto auto;
  gap: 12px;
  max-width: 500px;
  padding: 12px;
  background: var(--bento-bg);
  font-family: 'Arial Black', sans-serif;
  border: 4px solid var(--bento-border);
  box-shadow: 12px 12px 0px rgba(0,0,0,0.1);
}

.cell {
  background: var(--bento-bg);
  border: 3px solid var(--bento-border);
  padding: 15px;
  box-shadow: 6px 6px 0px var(--bento-border);
  display: flex;
  flex-direction: column;
  transition: transform 0.1s ease;
}

.mainCell {
  grid-column: span 3;
  flex-direction: row;
  align-items: center;
  gap: 15px;
  /* Uses the dynamic accent color */
  background: var(--current-accent);
}

.iconWrapper {
  width: 40px;
  height: 40px;
  background: var(--bento-border);
  color: var(--bento-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
}

.label {
  font-size: 0.6rem;
  color: var(--bento-border);
  opacity: 0.7;
  display: block;
}

.title {
  margin: 0;
  font-size: 1.2rem;
  text-transform: uppercase;
  color: var(--bento-border);
}

.contentCell {
  grid-column: span 4;
}

.description {
  margin: 0;
  font-family: 'Arial', sans-serif;
  font-weight: 600;
  font-size: 0.9rem;
  color: var(--bento-border);
}

.actionCell {
  grid-column: span 3;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  background: var(--bento-border);
  color: var(--bento-bg);
}

.actionCell:hover {
  transform: translate(-2px, -2px);
  /* The shadow on hover now uses the dynamic accent color */
  box-shadow: 8px 8px 0px var(--current-accent);
}

.closeCell {
  grid-column: span 1;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  background: var(--bento-bg);
  color: var(--bento-border);
  border: 3px solid var(--bento-border);
}

.closeCell:hover {
  background: var(--bento-border);
  color: var(--bento-bg);
}

@media (max-width: 400px) {
  .bentoAlert { grid-template-columns: repeat(2, 1fr); }
  .mainCell { grid-column: span 2; }
  .contentCell { grid-column: span 2; }
}
</style>