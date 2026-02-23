<template>
  <div v-bind="$attrs" :class="$style.speedRoot" :style="speedStyles">
    <div :class="$style.speedBar">
      
      <div :class="$style.gearSection">
        <div 
          v-for="g in gears" 
          :key="g"
          :class="[$style.skewBox, activeGear === g && $style.activeGear]"
        >
          <span :class="$style.gearLetter">{{ g }}</span>
        </div>
      </div>

      <div v-if="showGauge" :class="[$style.section, $style.flexGrow]">
        <div :class="$style.gaugeLabel">
          <span :class="$style.italicText">{{ gaugeLabel }}</span>
          <span :class="$style.valueText">{{ loadValue }}%</span>
        </div>
        <div :class="$style.tachoTrack">
          <div 
            v-for="i in steps" 
            :key="i" 
            :class="[
              $style.tachoStep, 
              (i/steps * 100) <= loadValue && $style.tachoStepActive,
              (i/steps * 100) > 85 && $style.redline
            ]"
          ></div>
        </div>
      </div>

      <div :class="$style.clockSection">
        <div :class="$style.timeWrapper">
          <span :class="$style.timeMain">{{ currentTime }}</span>
          <span :class="$style.unit">{{ unitTag }}</span>
        </div>
        <div v-if="nitro" :class="$style.nitroLine"></div>
      </div>
      
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  gaugeLabel?: string;
  loadValue?: number;
  activeGear?: string;   /* P, R, N, D, S */
  unitTag?: string;
  color?: string;        /* Color principal (Fuego/Nitro) */
  // Performance Tuning
  slant?: number;        /* Grados de inclinación (0 a -20) */
  steps?: number;        /* Cantidad de bloques en el tacómetro */
  nitro?: boolean;       /* ¿Línea de flujo animada? */
  showGauge?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  gaugeLabel: 'CORE_TEMP',
  loadValue: 74,
  activeGear: 'D',
  unitTag: 'GMT',
  color: '#ff3e00',
  slant: -10,
  steps: 20,
  nitro: true,
  showGauge: true
});

const gears = ['R', 'N', 'D', 'S'];
const currentTime = ref('00:00');
let timer: any = null;

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-GB', { 
    hour: '2-digit', minute: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});

onUnmounted(() => clearInterval(timer));

const speedStyles = computed(() => ({
  '--s-accent': props.color,
  '--s-slant': `${props.slant}deg`,
  '--s-slant-corr': `${props.slant * -1}deg`,
  '--s-bg': '#050505',
  '--s-step-count': props.steps
}));
</script>

<style module>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&display=swap');

.speedRoot {
  width: fit-content;
  padding: 15px;
  background: transparent;
}

.speedBar {
  display: flex;
  align-items: center;
  gap: 15px;
  background: var(--s-bg);
  padding: 6px 6px 6px 20px;
  border-left: 6px solid var(--s-accent);
  transform: skewX(var(--s-slant));
  box-shadow: 12px 12px 0px rgba(0,0,0,0.3);
  font-family: 'Orbitron', sans-serif;
  border-radius: 2px;
}

.section { display: flex; flex-direction: column; gap: 4px; }
.flexGrow { flex-grow: 1; min-width: 260px; }

/* Selector de Marchas */
.gearSection { display: flex; gap: 4px; }
.skewBox {
  background: #1a1a1a;
  padding: 4px 12px;
  border: 1px solid #222;
  transition: all 0.3s ease;
}
.activeGear {
  background: var(--s-accent);
  color: #000;
  box-shadow: 0 0 20px color-mix(in srgb, var(--s-accent), transparent 40%);
  border-color: var(--s-accent);
}
.gearLetter {
  display: block;
  transform: skewX(var(--s-slant-corr));
  font-size: 0.9rem;
  font-weight: 900;
}

/* Tacómetro Racing */
.gaugeLabel {
  display: flex;
  justify-content: space-between;
  transform: skewX(var(--s-slant-corr));
  padding-right: 5px;
}
.italicText { font-size: 0.6rem; color: #555; font-style: italic; letter-spacing: 1px; }
.valueText { font-size: 0.85rem; color: #fff; font-weight: 900; }

.tachoTrack {
  display: flex;
  gap: 3px;
  height: 14px;
}
.tachoStep {
  flex: 1;
  background: #1a1a1a;
  transform: skewX(-5deg);
  transition: background 0.1s ease;
}
.tachoStepActive {
  background: var(--s-accent);
  box-shadow: 0 0 10px var(--s-accent);
}

.redline.tachoStepActive {
  background: #ff0000;
  box-shadow: 0 0 15px #ff0000;
  animation: flicker 0.1s infinite;
}

@keyframes flicker {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.8; }
}

/* Reloj HUD */
.clockSection {
  background: #111;
  padding: 12px 30px;
  position: relative;
  overflow: hidden;
  border-right: 3px solid var(--s-accent);
}
.timeWrapper {
  transform: skewX(var(--s-slant-corr));
  display: flex;
  align-items: baseline;
  gap: 6px;
}
.timeMain { font-size: 1.5rem; font-weight: 900; color: #fff; letter-spacing: -1px; }
.unit { font-size: 0.65rem; color: var(--s-accent); font-weight: 900; }

.nitroLine {
  position: absolute;
  bottom: 0; left: 0;
  height: 3px; width: 100%;
  background: linear-gradient(90deg, transparent, var(--s-accent), #fff);
  animation: nitroFlow 0.8s infinite linear;
}

@keyframes nitroFlow {
  from { transform: translateX(-100%); }
  to { transform: translateX(100%); }
}
</style>