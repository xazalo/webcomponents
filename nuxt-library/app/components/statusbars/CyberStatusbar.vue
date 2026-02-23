<template>
  <div v-bind="$attrs" :class="$style.cyberRoot" :style="cyberStyles">
    <div v-if="showGrid" :class="$style.gridOverlay"></div>

    <div :class="$style.cell">
      <div :class="[$style.statusGroup, animated && $style.glitchAnim]">
        <div :class="$style.node"></div>
        <div :class="$style.stack">
          <span :class="$style.microLabel">NETWORK_LINK</span>
          <span :class="$style.cyberText">{{ statusText }}</span>
        </div>
      </div>
      <div :class="$style.cornerTrim"></div>
    </div>

    <div v-if="showLoad" :class="[$style.cell, $style.flexGrow]">
      <div :class="$style.dataHeader">
        <span :class="$style.microLabel">{{ loadLabel }}</span>
        <span :class="$style.valueText">{{ loadValue }}%</span>
      </div>
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: loadValue + '%' }">
          <div v-if="animated" :class="$style.scanline"></div>
        </div>
      </div>
    </div>

    <div :class="[$style.cell, $style.clockCell]">
      <div :class="$style.timeWrapper">
        <span :class="$style.timeMain">{{ currentTime.split(':')[0] }}:{{ currentTime.split(':')[1] }}</span>
        <span :class="$style.timeSec">{{ currentTime.split(':')[2] }}</span>
      </div>
      <div :class="$style.decoLines"></div>
      <div :class="$style.angleCut"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  statusText?: string;
  loadLabel?: string;
  loadValue?: number;
  color?: string;       /* Color Neon (Cian, Magenta, Lima) */
  // Cyber Tuning
  glowIntensity?: number; /* 0 a 20px */
  gridOpacity?: number;   /* 0 a 0.3 */
  showGrid?: boolean;
  showLoad?: boolean;
  animated?: boolean;     /* ¿Activar escaneo y glitches? */
}

const props = withDefaults(defineProps<Props>(), {
  statusText: 'UPLINK_STABLE',
  loadLabel: 'VOLTAGE_LOAD',
  loadValue: 68,
  color: '#00f3ff',
  glowIntensity: 10,
  gridOpacity: 0.1,
  showGrid: true,
  showLoad: true,
  animated: true
});

const currentTime = ref('00:00:00');
let timer: any = null;

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-GB', { 
    hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});

onUnmounted(() => clearInterval(timer));

const cyberStyles = computed(() => ({
  '--c-neon': props.color,
  '--c-glow': `${props.glowIntensity}px`,
  '--c-grid-op': props.gridOpacity,
  '--c-border': `color-mix(in srgb, ${props.color} 30%, transparent)`,
}));
</script>

<style module>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

.cyberRoot {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: #0a0a0f;
  border: 1px solid var(--c-border);
  position: relative;
  font-family: 'Share Tech Mono', monospace;
  overflow: hidden;
  width: fit-content;
  box-shadow: inset 0 0 20px rgba(0,0,0,0.5);
}

.gridOverlay {
  position: absolute; inset: 0;
  background-image: linear-gradient(var(--c-border) 1px, transparent 1px),
                    linear-gradient(90deg, var(--c-border) 1px, transparent 1px);
  background-size: 15px 15px;
  opacity: var(--c-grid-op);
  pointer-events: none;
}

.cell {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid var(--c-border);
  padding: 8px 15px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  z-index: 1;
}

.flexGrow { flex-grow: 1; min-width: 200px; }

.statusGroup { display: flex; align-items: center; gap: 12px; }
.stack { display: flex; flex-direction: column; }

.node {
  width: 10px; height: 10px;
  background: var(--c-neon);
  box-shadow: 0 0 var(--c-glow) var(--c-neon);
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
}

.microLabel { font-size: 0.55rem; color: #666; letter-spacing: 2px; font-weight: bold; }
.cyberText { color: var(--c-neon); font-size: 0.8rem; text-shadow: 0 0 5px var(--c-neon); }

/* --- Barra de Energía --- */
.dataHeader { display: flex; justify-content: space-between; margin-bottom: 4px; }
.valueText { font-size: 0.85rem; color: #fff; font-weight: bold; }

.track {
  height: 8px; background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 2px; position: relative;
}

.fill {
  height: 100%; background: var(--c-neon);
  box-shadow: 0 0 var(--c-glow) var(--c-neon);
  position: relative; transition: width 0.4s cubic-bezier(0.23, 1, 0.32, 1);
}

.scanline {
  position: absolute; inset: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.6), transparent);
  width: 30%; animation: scan 2s infinite linear;
}

@keyframes scan { from { left: -100%; } to { left: 150%; } }

/* --- Reloj --- */
.clockCell {
  background: color-mix(in srgb, var(--c-neon) 10%, transparent);
  min-width: 120px; border-left: 3px solid var(--c-neon);
}

.timeWrapper { display: flex; align-items: baseline; justify-content: center; color: var(--c-neon); }
.timeMain { font-size: 1.3rem; font-weight: bold; }
.timeSec { font-size: 0.75rem; margin-left: 4px; opacity: 0.8; }

/* --- Estética Glitch y Cortes --- */
.cornerTrim {
  position: absolute; top: 0; right: 0;
  width: 12px; height: 12px;
  background: #0a0a0f;
  clip-path: polygon(100% 0, 0 0, 100% 100%);
  border-left: 1px solid var(--c-border);
}

.decoLines {
  position: absolute; bottom: 0; left: 0; right: 0;
  height: 4px; opacity: 0.2;
  background: repeating-linear-gradient(90deg, var(--c-neon) 0 2px, transparent 2px 5px);
}

.glitchAnim { animation: glitch 4s infinite; }

@keyframes glitch {
  0%, 90%, 100% { transform: none; opacity: 1; }
  92% { transform: skewX(10deg); opacity: 0.8; }
  94% { transform: skewX(-15deg); filter: hue-rotate(90deg); }
  96% { transform: translate(2px, -1px); }
}
</style>