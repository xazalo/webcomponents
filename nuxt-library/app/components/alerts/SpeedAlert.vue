<template>
  <div :class="[$style.speedAlert, $style[type]]">
    <div :class="$style.nitroBar"></div>

    <div :class="$style.mainContent">
      <div :class="$style.header">
        <div :class="$style.iconHex">
          <slot name="icon">!</slot>
        </div>
        <div :class="$style.titleWrapper">
          <span :class="$style.topLabel">// SECTOR_01 //</span>
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
defineProps<{
  title: string;
  message: string;
  type: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  buttonText: string;
}>();

defineEmits(['close', 'action']);
</script>

<style module>
:root {
  --speed-red: #ff0000;
  --speed-black: #0a0a0a;
  --speed-gray: #1a1a1a;
  --speed-white: #ffffff;
  --speed-accent: #00ff41; /* Verde Nitro */
}

.speedAlert {
  display: flex;
  max-width: 460px;
  background: var(--speed-gray);
  position: relative;
  overflow: hidden;
  /* Inclinación aerodinámica */
  transform: skewX(-6deg);
  border: 2px solid #333;
  box-shadow: 15px 15px 0px rgba(0,0,0,0.2);
  
  /* Textura de fibra de carbono */
  background-image: 
    linear-gradient(45deg, #151515 25%, transparent 25%), 
    linear-gradient(-45deg, #151515 25%, transparent 25%), 
    linear-gradient(45deg, transparent 75%, #151515 75%), 
    linear-gradient(-45deg, transparent 75%, #151515 75%);
  background-size: 4px 4px;
  background-color: #1a1a1a;
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
  /* Anulamos el skew para el texto */
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

.titleWrapper {
  display: flex;
  flex-direction: column;
}

.topLabel {
  font-family: 'Arial Black', sans-serif;
  font-size: 0.6rem;
  color: var(--type-color);
  letter-spacing: 2px;
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
  color: #bbb;
  border-left: 2px solid #333;
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
  color: #666;
  border: 1px solid #444;
  padding: 12px;
  font-weight: 700;
  font-style: italic;
  cursor: pointer;
}

.closeBtn:hover {
  color: var(--speed-white);
  border-color: var(--speed-white);
}

.checkeredPattern {
  position: absolute;
  top: 0; right: -20px;
  width: 40px; height: 100%;
  background-image: conic-gradient(#222 90deg, #111 90deg 180deg, #222 180deg 270deg, #111 270deg);
  background-size: 10px 10px;
  opacity: 0.3;
  transform: skewX(-15deg);
}

/* Variantes de Color de Competición */
.info { --type-color: #00e5ff; }
.warning { --type-color: #ffcc00; }
.error { --type-color: #ff3e3e; }
.success { --type-color: var(--speed-accent); }
</style>