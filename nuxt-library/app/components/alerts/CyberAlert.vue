<template>
  <div :class="[$style.cyberAlert, $style[type]]" :style="dynamicStyles">
    <div :class="$style.scanline"></div>
    
    <div :class="$style.cornerDeco"></div>
    <div :class="$style.edgeTag">VS: ALERT_SYS_v2.0</div>

    <div :class="$style.header">
      <div 
        :class="[$style.glitchIcon, { [$style.errorAnim]: type === 'error' }]" 
        :data-text="type === 'error' ? '✖' : '▲'"
      >
        <slot name="icon">{{ type === 'error' ? '✖' : '▲' }}</slot>
      </div>
      <div :class="$style.titleStack">
        <span :class="$style.subLabel">{{ statusText || 'SYSTEM MESSAGE' }}</span>
        <h3 :class="$style.title">{{ title }}</h3>
      </div>
    </div>

    <div :class="$style.body">
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footer">
      <button :class="$style.actionBtn" @click="$emit('action')">
        <span :class="$style.btnCut"></span>
        <span :class="$style.btnText">{{ actionText }}</span>
      </button>
      <button :class="$style.closeBtn" @click="$emit('close')">
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
  statusText?: string;
  buttonText: string;
  // Dynamic color props
  neonColor?: string;
  bgColor?: string;
  glitchSecondary?: string;
}

const props = withDefaults(defineProps<Props>(), {
  bgColor: '#050505',
  glitchSecondary: '#ff003c' // Pink glitch default
});

defineEmits(['close', 'action']);

const dynamicStyles = computed(() => {
  const neonPalette = {
    info: '#00f0ff',   // Cyber Blue
    warning: '#fcee0a', // Cyber Yellow
    error: '#ff003c',   // Cyber Pink
    success: '#39ff14'  // Neon Green
  };

  return {
    '--current-neon': props.neonColor || neonPalette[props.type],
    '--cb-black': props.bgColor,
    '--glitch-color': props.glitchSecondary,
  };
});
</script>

<style module>
.cyberAlert {
  display: flex;
  flex-direction: column;
  padding: 25px;
  max-width: 450px;
  background: var(--cb-black);
  border-left: 5px solid var(--current-neon);
  position: relative;
  /* Aggressive bottom-right cut */
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 20px), calc(100% - 20px) 100%, 0 100%);
  font-family: 'Electrolize', 'Orbitron', sans-serif;
  color: #fff;
  overflow: hidden;
}

.scanline {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(255, 255, 255, 0.02) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.cornerDeco {
  position: absolute;
  top: 0; right: 0;
  width: 40px; height: 4px;
  background: var(--current-neon);
  box-shadow: 0 0 15px var(--current-neon);
}

.edgeTag {
  position: absolute;
  bottom: 5px; right: 25px;
  font-size: 10px;
  color: var(--current-neon);
  font-family: monospace;
  opacity: 0.7;
}

.header {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 20px;
}

.glitchIcon {
  font-size: 2rem;
  font-weight: 900;
  color: var(--current-neon);
  position: relative;
  /* Use the glitch prop for the secondary shadow */
  text-shadow: 2px 0 var(--glitch-color), -2px 0 #00f0ff;
}

.subLabel {
  font-size: 0.6rem;
  letter-spacing: 2px;
  color: var(--current-neon);
  opacity: 0.8;
  display: block;
}

.title {
  margin: 0;
  font-size: 1.6rem;
  text-transform: uppercase;
  letter-spacing: -1px;
}

.body {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 15px 0;
  margin-bottom: 10px;
}

.message {
  margin: 0;
  font-size: 1rem;
  color: #ccc;
  line-height: 1.4;
  font-family: 'Consolas', monospace;
}

.footer {
  display: flex;
  gap: 15px;
}

.actionBtn {
  flex: 1;
  background: var(--current-neon);
  color: #000;
  border: none;
  padding: 12px;
  font-weight: 900;
  text-transform: uppercase;
  cursor: pointer;
  position: relative;
  clip-path: polygon(0 0, 100% 0, 90% 100%, 0 100%);
  transition: all 0.2s;
}

.actionBtn:hover {
  filter: brightness(1.2);
  transform: translateX(5px);
  box-shadow: -5px 0 0 var(--glitch-color);
}

.closeBtn {
  background: transparent;
  color: var(--current-neon);
  border: 1px solid var(--current-neon);
  padding: 12px 20px;
  font-weight: 700;
  cursor: pointer;
  clip-path: polygon(10% 0, 100% 0, 100% 100%, 0 100%);
  transition: background 0.2s;
}

.closeBtn:hover {
  background: rgba(255, 255, 255, 0.1);
}

.errorAnim {
  animation: glitch-anim 2s infinite;
}

@keyframes glitch-anim {
  0% { transform: translate(0); }
  20% { transform: translate(-2px, 2px); }
  40% { transform: translate(-2px, -2px); }
  60% { transform: translate(2px, 2px); }
  80% { transform: translate(2px, -2px); }
  100% { transform: translate(0); }
}
</style>