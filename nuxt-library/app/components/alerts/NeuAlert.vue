<template>
  <div :class="[$style.neuAlert, $style[type]]" :style="dynamicStyles">
    <div :class="$style.iconInverted">
      <div :class="$style.iconInner">
        <slot name="icon">!</slot>
      </div>
    </div>

    <div :class="$style.content">
      <span :class="$style.typeLabel">{{ type.toUpperCase() }}</span>
      <h3 :class="$style.title">{{ title }}</h3>
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footer">
      <button :class="$style.confirmBtn" @click="$emit('action')">
        {{ actionText }}
      </button>
      <button :class="$style.cancelBtn" @click="$emit('close')">
        {{ buttonText }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title: string;
  message: string;
  type: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  buttonText: string;
  // Neumorphic core props
  baseBg?: string;      // The main surface color
  lightShadow?: string; // The highlight (usually white)
  darkShadow?: string;  // The depth shadow
  accentColor?: string; // Icon and button text color
}

const props = withDefaults(defineProps<Props>(), {
  baseBg: '#e0e5ec',
  lightShadow: '#ffffff',
  darkShadow: '#a3b1c6',
});

defineEmits(['close', 'action']);

const dynamicStyles = computed(() => {
  const typeColors = {
    info: '#6d5dfc',
    warning: '#fab1a0',
    error: '#ff7675',
    success: '#55efc4'
  };

  return {
    '--neu-bg': props.baseBg,
    '--neu-light': props.lightShadow,
    '--neu-shadow': props.darkShadow,
    '--type-accent': props.accentColor || typeColors[props.type],
  };
});
</script>

<style module>
.neuAlert {
  display: flex;
  flex-direction: column;
  padding: 35px;
  max-width: 420px;
  background: var(--neu-bg);
  border-radius: 50px;
  
  /* Extrusion: Light top-left, Shadow bottom-right */
  box-shadow: 
    9px 9px 16px var(--neu-shadow), 
    -9px -9px 16px var(--neu-light);
  
  font-family: 'Segoe UI', system-ui, sans-serif;
  transition: all 0.3s ease;
}

.iconInverted {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  
  /* Inset (sunken) effect */
  background: var(--neu-bg);
  box-shadow: 
    inset 6px 6px 10px var(--neu-shadow), 
    inset -6px -6px 10px var(--neu-light);
}

.iconInner {
  font-size: 1.5rem;
  font-weight: 900;
  color: var(--type-accent);
  text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
}

.typeLabel {
  font-size: 0.7rem;
  font-weight: 800;
  color: var(--neu-shadow);
  letter-spacing: 1.5px;
}

.title {
  margin: 5px 0 12px;
  font-size: 1.6rem;
  color: #44475a;
  font-weight: 700;
}

.message {
  margin: 0 0 25px;
  font-size: 1rem;
  color: #7a8ba9;
  line-height: 1.6;
}

.footer {
  display: flex;
  gap: 20px;
}

.confirmBtn {
  flex: 1;
  padding: 15px;
  border-radius: 20px;
  border: none;
  background: var(--neu-bg);
  color: var(--type-accent);
  font-weight: 700;
  cursor: pointer;
  
  /* Raised effect */
  box-shadow: 
    5px 5px 10px var(--neu-shadow), 
    -5px -5px 10px var(--neu-light);
  transition: all 0.2s ease;
}

.confirmBtn:active {
  /* Pressed (sunken) effect */
  box-shadow: 
    inset 3px 3px 6px var(--neu-shadow), 
    inset -3px -3px 6px var(--neu-light);
  transform: scale(0.98);
}

.cancelBtn {
  flex: 1;
  padding: 15px;
  background: transparent;
  border: none;
  color: #7a8ba9;
  font-weight: 600;
  cursor: pointer;
  transition: color 0.2s;
}

.cancelBtn:hover {
  color: #44475a;
}
</style>