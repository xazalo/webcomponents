<template>
  <div :class="[$style.speedAlert, $style[type]]" :style="dynamicStyles">
    <div :class="$style.nitroBar"></div>

    <div :class="$style.mainContent">
      <div :class="$style.header">
        <div :class="$style.iconHex">
          <slot name="icon">!</slot>
        </div>
        <div :class="$style.titleWrapper">
          <span :class="$style.topLabel">// {{ sectorLabel }} //</span>
          <h3 :class="$style.title">{{ title }}</h3>
        </div>
      </div>

      <div :class="$style.body">
        <p :class="$style.message">{{ message }}</p>
      </div>

      <div :class="$style.footer">
        <button :class="$style.actionBtn" @click="$emit('action')">
          <span :class="$style.btnContent">{{ actionText }}</span>
        </button>
        <button :class="$style.closeBtn" @click="$emit('close')">
          {{ buttonText }}
        </button>
      </div>
    </div>

    <div :class="$style.checkeredPattern"></div>
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
  sectorLabel?: string;
  // Customization Props
  accentColor?: string;
  primaryBg?: string;
  textColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  sectorLabel: 'SECTOR_01',
  primaryBg: '#1a1a1a',
  textColor: '#ffffff'
});

defineEmits(['close', 'action']);

const dynamicStyles = computed(() => {
  const racingPalette = {
    info: '#00e5ff',
    warning: '#ffcc00',
    error: '#ff3e3e',
    success: '#00ff41'
  };

  return {
    '--type-color': props.accentColor || racingPalette[props.type],
    '--speed-gray': props.primaryBg,
    '--speed-white': props.textColor,
  };
});
</script>

<style module>
.speedAlert {
  display: flex;
  max-width: 460px;
  background: var(--speed-gray);
  position: relative;
  overflow: hidden;
  /* Aerodynamic Skew */
  transform: skewX(-6deg);
  border: 2px solid rgba(255, 255, 255, 0.1);
  box-shadow: 15px 15px 0px rgba(0,0,0,0.2);
  
  /* Carbon Fiber Texture */
  background-image: 
    linear-gradient(45deg, rgba(0,0,0,0.2) 25%, transparent 25%), 
    linear-gradient(-45deg, rgba(0,0,0,0.2) 25%, transparent 25%), 
    linear-gradient(45deg, transparent 75%, rgba(0,0,0,0.2) 75%), 
    linear-gradient(-45deg, transparent 75%, rgba(0,0,0,0.2) 75%);
  background-size: 4px 4px;
  background-color: var(--speed-gray);
}

.nitroBar {
  width: 8px;
  background: var(--type-color);
  box-shadow: 0 0 20px var(--type-color);
  z-index: 2;
}

.mainContent {
  padding: 25px;
  flex: 1;
  /* Reverse the skew for text readability */
  transform: skewX(6deg);
}

.header {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 20px;
}

.iconHex {
  width: 40px;
  height: 40px;
  background: var(--type-color);
  clip-path: polygon(25% 0%, 75% 0%, 100% 50%, 75% 100%, 25% 100%, 0% 50%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #000;
  font-weight: 900;
  font-size: 1.2rem;
}

.topLabel {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.6rem;
  color: var(--type-color);
  letter-spacing: 2px;
  display: block;
}

.title {
  margin: 0;
  font-size: 1.6rem;
  font-weight: 900;
  font-style: italic;
  color: var(--speed-white);
  text-transform: uppercase;
}

.message {
  font-family: 'Courier New', monospace;
  font-size: 0.95rem;
  color: var(--speed-white);
  opacity: 0.7;
  border-left: 2px solid var(--type-color);
  padding-left: 15px;
  margin: 0 0 20px;
}

.footer {
  display: flex;
  gap: 10px;
}

.actionBtn {
  flex: 2;
  background: var(--speed-white);
  color: #000;
  border: none;
  padding: 12px;
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
  clip-path: polygon(0 0, 100% 0, 95% 100%, 0 100%);
  transition: all 0.2s;
}

.actionBtn:hover {
  background: var(--type-color);
  transform: translateX(5px);
}

.closeBtn {
  flex: 1;
  background: transparent;
  color: var(--speed-white);
  opacity: 0.6;
  border: 1px solid currentColor;
  padding: 12px;
  font-weight: 700;
  font-style: italic;
  cursor: pointer;
  transition: opacity 0.2s;
}

.closeBtn:hover {
  opacity: 1;
}

.checkeredPattern {
  position: absolute;
  top: 0; right: -20px;
  width: 40px; height: 100%;
  background-image: conic-gradient(
    rgba(255,255,255,0.1) 90deg, 
    transparent 90deg 180deg, 
    rgba(255,255,255,0.1) 180deg 270deg, 
    transparent 270deg
  );
  background-size: 10px 10px;
  transform: skewX(-15deg);
}
</style>