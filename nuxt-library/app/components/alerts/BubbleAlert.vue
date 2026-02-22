<template>
  <div :class="[$style.bubbleAlert, $style[type]]">
    <div :class="$style.mainGlare"></div>

    <div :class="$style.header">
      <div :class="$style.iconCircle">
        <slot name="icon">!</slot>
      </div>
      <h3 :class="$style.title">{{ title }}</h3>
    </div>

    <div :class="$style.body">
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.actions">
      <button :class="$style.actionBtn" @click="$emit('action')">
        {{ actionText }}
      </button>
      <button :class="$style.closeBtn" @click="$emit('close')">
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
  buttonText: string
}>();

defineEmits(['close', 'action']);
</script>

<style module>
:root {
  --bubble-bg-info: #70d6ff;
  --bubble-bg-warning: #ffd670;
  --bubble-bg-error: #ff70a6;
  --bubble-bg-success: #8dffbe;
}

.bubbleAlert {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding: 25px;
  max-width: 400px;
  border-radius: 40px;
  position: relative;
  overflow: hidden;
  
  /* Gradiente base + Sombra de "inflado" */
  background: var(--current-color);
  box-shadow: 
    0 20px 40px rgba(0, 0, 0, 0.1),
    inset -8px -8px 20px rgba(0, 0, 0, 0.1),
    inset 8px 8px 20px rgba(255, 255, 255, 0.5);
  
  font-family: 'Rounded Mplus 1c', 'Quicksand', sans-serif;
  transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.bubbleAlert:hover {
  transform: scale(1.02);
}

.mainGlare {
  position: absolute;
  top: 0;
  left: 10%;
  width: 80%;
  height: 50%;
  background: linear-gradient(to bottom, rgba(255,255,255,0.4), transparent);
  border-radius: 40px 40px 100% 100%;
  pointer-events: none;
}

.header {
  display: flex;
  align-items: center;
  gap: 15px;
}

.iconCircle {
  width: 45px;
  height: 45px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 900;
  font-size: 1.5rem;
  color: var(--current-color);
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.title {
  margin: 0;
  font-size: 1.4rem;
  font-weight: 800;
  color: #2d3436;
}

.body {
  background: rgba(255, 255, 255, 0.3);
  padding: 15px;
  border-radius: 25px;
  backdrop-filter: blur(5px);
}

.message {
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
  color: #2d3436;
  line-height: 1.4;
}

.actions {
  display: flex;
  gap: 10px;
}

.actionBtn {
  flex: 2;
  background: #2d3436;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 20px;
  font-weight: 800;
  cursor: pointer;
  transition: all 0.2s ease;
}

.actionBtn:hover {
  background: #000;
  transform: translateY(-2px);
}

.closeBtn {
  flex: 1;
  background: rgba(255, 255, 255, 0.5);
  color: #2d3436;
  border: none;
  padding: 12px;
  border-radius: 20px;
  font-weight: 700;
  cursor: pointer;
}

.closeBtn:hover {
  background: white;
}

/* Variantes de color */
.info { --current-color: var(--bubble-bg-info); }
.warning { --current-color: var(--bubble-bg-warning); }
.error { --current-color: var(--bubble-bg-error); }
.success { --current-color: var(--bubble-bg-success); }
</style>