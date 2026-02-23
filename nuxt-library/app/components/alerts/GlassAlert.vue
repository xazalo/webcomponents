<template>
  <div :class="[$style.glassAlert, $style[type]]" :style="dynamicStyles">
    <div :class="$style.glow"></div>
    
    <div :class="$style.header">
      <div :class="$style.iconAmbient">
        <slot name="icon">!</slot>
      </div>
      <div :class="$style.titleGroup">
        <h3 :class="$style.title">{{ title }}</h3>
      </div>
    </div>

    <div :class="$style.body">
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footer">
      <button :class="$style.primaryBtn" @click="$emit('action')">
        {{ actionText }}
      </button>
      <button :class="$style.secondaryBtn" @click="$emit('close')">
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
  // Prop-driven customization
  accentColor?: string;
  glassColor?: string;
  textColor?: string;
  blurAmount?: string;
}

const props = withDefaults(defineProps<Props>(), {
  glassColor: 'rgba(255, 255, 255, 0.15)',
  textColor: '#ffffff',
  blurAmount: '15px'
});

defineEmits(['close', 'action']);

const dynamicStyles = computed(() => {
  const typeColors = {
    info: '#00d1ff',
    warning: '#ffb800',
    error: '#ff4b4b',
    success: '#00ff66'
  };

  const selectedAccent = props.accentColor || typeColors[props.type];

  return {
    '--type-color': selectedAccent,
    '--glass-bg': props.glassColor,
    '--glass-white': props.textColor,
    '--blur-val': props.blurAmount,
    /* Auto-generate border color based on text color opacity */
    '--glass-border': props.textColor.includes('rgba') 
      ? props.textColor 
      : `${props.textColor}66` // Appends 40% hex alpha if it's a standard hex
  };
});
</script>

<style module>
.glassAlert {
  display: flex;
  flex-direction: column;
  padding: 30px;
  max-width: 440px;
  position: relative;
  overflow: hidden;
  
  /* Core Glassmorphism Logic */
  background: var(--glass-bg);
  backdrop-filter: blur(var(--blur-val)) saturate(160%);
  -webkit-backdrop-filter: blur(var(--blur-val)) saturate(160%);
  
  border-radius: 28px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
  
  font-family: 'Inter', system-ui, sans-serif;
  color: var(--glass-white);
}

.glow {
  position: absolute;
  top: -20%;
  left: -20%;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, var(--type-color) 0%, transparent 70%);
  opacity: 0.2;
  pointer-events: none;
}

.header {
  display: flex;
  align-items: center;
  gap: 18px;
  margin-bottom: 20px;
  position: relative;
}

.iconAmbient {
  width: 44px;
  height: 44px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 1.2rem;
  box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.05);
}

.title {
  margin: 0;
  font-size: 1.4rem;
  font-weight: 600;
  letter-spacing: -0.5px;
  color: var(--glass-white);
}

.body {
  margin-bottom: 25px;
  position: relative;
}

.message {
  margin: 0;
  font-size: 1rem;
  line-height: 1.5;
  color: var(--glass-white);
  opacity: 0.85;
}

.footer {
  display: flex;
  gap: 12px;
  position: relative;
}

.primaryBtn {
  flex: 2;
  background: var(--glass-white);
  color: #000;
  border: none;
  padding: 12px;
  border-radius: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, filter 0.2s;
}

.primaryBtn:hover {
  transform: translateY(-2px);
  filter: brightness(0.9);
}

.secondaryBtn {
  flex: 1;
  background: rgba(255, 255, 255, 0.1);
  color: var(--glass-white);
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 12px;
  border-radius: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.secondaryBtn:hover {
  background: rgba(255, 255, 255, 0.2);
}

/* Side highlight based on type */
.glassAlert::before {
  content: '';
  position: absolute;
  top: 0; 
  left: 0; 
  width: 4px; 
  height: 100%;
  background: var(--type-color);
  box-shadow: 0 0 15px var(--type-color);
  opacity: 0.8;
  z-index: 2;
}
</style>