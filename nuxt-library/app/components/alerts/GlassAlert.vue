<template>
  <div :class="[$style.glassAlert, $style[type]]">
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
  --glass-white: rgba(255, 255, 255, 1);
  --glass-border: rgba(255, 255, 255, 0.4);
  --glass-bg: rgba(255, 255, 255, 0.15);
}
J
.glassAlert {
  display: flex;
  flex-direction: column;
  padding: 30px;
  max-width: 440px;
  position: relative;
  overflow: hidden;
  
  /* El corazón del Glassmorphism */
  background: var(--glass-bg);
  backdrop-filter: blur(15px) saturate(160%);
  -webkit-backdrop-filter: blur(15px) saturate(160%);
  
  border-radius: 28px;
  border: 1px solid var(--glass-border);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
  
  font-family: 'Inter', system-ui, sans-serif;
  color: var(--glass-white);
}

/* Un gradiente interno que le da profundidad de color según el tipo */
.glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, var(--type-color) 0%, transparent 70%);
  opacity: 0.15;
  pointer-events: none;
}

.header {
  display: flex;
  align-items: center;
  gap: 18px;
  margin-bottom: 20px;
}

.iconAmbient {
  width: 44px;
  height: 44px;
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid var(--glass-border);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 1.2rem;
  box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.1);
}

.systemLabel {
  font-size: 0.65rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  font-weight: 700;
  opacity: 0.6;
}

.title {
  margin: 2px 0 0;
  font-size: 1.4rem;
  font-weight: 600;
  letter-spacing: -0.5px;
}

.body {
  margin-bottom: 25px;
}

.message {
  margin: 0;
  font-size: 1rem;
  line-height: 1.5;
  color: rgba(255, 255, 255, 0.85);
  font-weight: 400;
}

.footer {
  display: flex;
  gap: 12px;
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
  transition: transform 0.2s, opacity 0.2s;
}

.primaryBtn:hover {
  transform: translateY(-2px);
  opacity: 0.9;
}

.secondaryBtn {
  flex: 1;
  background: rgba(255, 255, 255, 0.1);
  color: var(--glass-white);
  border: 1px solid var(--glass-border);
  padding: 12px;
  border-radius: 14px;
  font-weight: 600;
  cursor: pointer;
}

.secondaryBtn:hover {
  background: rgba(255, 255, 255, 0.2);
}

/* Variantes de Color Atmosférico */
.info { --type-color: #00d1ff; }
.warning { --type-color: #ffb800; }
.error { --type-color: #ff4b4b; }
.success { --type-color: #00ff66; }

/* Borde dinámico según el tipo */
.glassAlert::before {
  content: '';
  position: absolute;
  top: 0; left: 0; width: 4px; height: 100%;
  background: var(--type-color);
  opacity: 0.8;
}
</style>