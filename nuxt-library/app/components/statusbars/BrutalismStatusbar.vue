<template>
  <div v-bind="$attrs" :class="$style.brutalRoot" :style="brutalStyles">
    <div :class="$style.panel">
      <div :class="[$style.dot, blinking && $style.blinkAnim]"></div>
      <div :class="$style.textGroup">
        <span :class="$style.title">{{ systemLabel }}</span>
        <span :class="$style.badge">{{ statusText }}</span>
      </div>
    </div>

    <div v-if="showLoad" :class="[$style.panel, $style.flexGrow]">
      <span :class="$style.label">LOAD_INDEX</span>
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: loadValue + '%' }"></div>
        <div :class="$style.ticks">
          <span v-for="i in 4" :key="i"></span>
        </div>
      </div>
      <span :class="$style.number">{{ loadValue }}%</span>
    </div>

    <div :class="$style.panel">
      <span :class="$style.label">SIGNAL</span>
      <div :class="$style.signalGrid">
        <div 
          v-for="i in 4" 
          :key="i" 
          :class="[$style.signalBar, i <= signalLevel && $style.signalActive]"
        ></div>
      </div>
    </div>

    <div :class="[$style.panel, $style.clockPanel]">
      <span :class="$style.timeText">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  systemLabel?: string;
  statusText?: string;
  loadValue?: number;
  signalLevel?: number; // 1 a 4
  color?: string;       /* Color de acento (Yellow/Magenta/Cyan) */
  // Brutal Props
  borderSize?: number;  /* Grosor de bordes (2-6px) */
  shadowSize?: number;  /* Desplazamiento de la sombra sólida */
  blinking?: boolean;   /* ¿Blink en el indicador? */
  showLoad?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  systemLabel: 'CORE_V8',
  statusText: 'ONLINE',
  loadValue: 62,
  signalLevel: 3,
  color: '#ffff00',
  borderSize: 3,
  shadowSize: 5,
  blinking: true,
  showLoad: true
});

const currentTime = ref('');
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

const brutalStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-border': `${props.borderSize}px`,
  '--b-shadow': `${props.shadowSize}px`,
  '--b-shadow-color': '#000000',
}));
</script>

<style module>
.brutalRoot {
  display: flex;
  gap: 12px;
  padding: 10px;
  background: transparent;
  font-family: 'Arial Black', 'Inter', sans-serif;
  width: fit-content;
}

.panel {
  background: #ffffff;
  border: var(--b-border) solid #000000;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 14px;
  box-shadow: var(--b-shadow) var(--b-shadow) 0px var(--b-shadow-color);
  transition: transform 0.1s;
}

.panel:hover {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--b-shadow) + 2px) calc(var(--b-shadow) + 2px) 0px var(--b-shadow-color);
}

.flexGrow { flex-grow: 1; min-width: 250px; }

.textGroup { display: flex; align-items: center; gap: 8px; }

.dot {
  width: 14px; height: 14px;
  background: #ff0000;
  border: calc(var(--b-border) - 1px) solid #000;
}

.blinkAnim { animation: blink 0.8s steps(2) infinite; }
@keyframes blink { to { opacity: 0; } }

.title { font-size: 0.85rem; font-weight: 900; letter-spacing: -0.5px; }

.badge {
  background: #000; color: #fff;
  padding: 2px 8px; font-size: 0.65rem;
  font-weight: 900; text-transform: uppercase;
}

.label { font-size: 0.75rem; font-weight: 900; text-transform: uppercase; color: #000; }

/* --- Barra de Progreso Brutal --- */
.track {
  height: 20px; flex-grow: 1;
  background: #fff; border: var(--b-border) solid #000;
  position: relative; overflow: hidden;
}

.fill {
  height: 100%; background: var(--b-accent);
  border-right: var(--b-border) solid #000;
}

.ticks {
  position: absolute; inset: 0;
  display: flex; justify-content: space-between;
  pointer-events: none; padding: 0 10%;
}

.ticks span { width: 2px; height: 100%; background: rgba(0,0,0,0.2); }

.number { font-size: 1rem; font-weight: 900; font-style: italic; }

/* --- Señal --- */
.signalGrid { display: flex; align-items: flex-end; gap: 4px; height: 16px; }

.signalBar {
  width: 6px; background: #fff;
  border: 2px solid #000;
}
.signalBar:nth-child(1) { height: 40%; }
.signalBar:nth-child(2) { height: 60%; }
.signalBar:nth-child(3) { height: 80%; }
.signalBar:nth-child(4) { height: 100%; }

.signalActive { background: #00ff00; }

/* --- Reloj --- */
.clockPanel {
  background: var(--b-accent);
  min-width: 140px; justify-content: center;
}

.timeText { font-size: 1.2rem; font-weight: 900; color: #000; letter-spacing: -1px; }
</style>