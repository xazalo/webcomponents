<template>
  <div :class="$style.cyberBar" :style="cyberStyles">
    <div :class="$style.gridOverlay"></div>

    <div :class="$style.cyberCell">
      <div :class="$style.glitchBox">
        <div :class="$style.statusNode"></div>
        <span :class="$style.cyberText">LINK: ACTIVE</span>
      </div>
      <div :class="$style.cornerTrim"></div>
    </div>

    <div :class="[$style.cyberCell, $style.flexGrow]">
      <div :class="$style.dataHeader">
        <span :class="$style.miniLabel">SYSTEM_LOAD</span>
        <span :class="$style.valueText">{{ load }}%</span>
      </div>
      <div :class="$style.energyTrack">
        <div :class="$style.energyFill" :style="{ width: load + '%' }">
          <div :class="$style.scanline"></div>
        </div>
      </div>
    </div>

    <div :class="[$style.cyberCell, $style.clockCell]">
      <div :class="$style.timeWrapper">
        <span :class="$style.timeMain">{{ currentTime.split(':')[0] }}:{{ currentTime.split(':')[1] }}</span>
        <span :class="$style.timeSec">{{ currentTime.split(':')[2] }}</span>
      </div>
      <div :class="$style.decoLines"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const props = defineProps<{ color?: string }>();
const load = ref(68);
const currentTime = ref('00:00:00');

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-GB', { 
    hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
});

const cyberStyles = computed(() => ({
  '--c-neon': props.color || '#00f3ff', // Cian cyber
  '--c-dark': '#0a0a0f',
  '--c-border': 'rgba(0, 243, 255, 0.3)'
}));
</script>

<style module>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

.cyberBar {
  display: flex;
  align-items: stretch;
  gap: 10px;
  padding: 8px;
  background: var(--c-dark);
  border: 1px solid var(--c-border);
  position: relative;
  font-family: 'Share Tech Mono', monospace;
  overflow: hidden;
  width: fit-content;
  min-width: 600px;
}

.gridOverlay {
  position: absolute;
  inset: 0;
  background-image: linear-gradient(var(--c-border) 1px, transparent 1px),
                    linear-gradient(90deg, var(--c-border) 1px, transparent 1px);
  background-size: 20px 20px;
  opacity: 0.1;
  pointer-events: none;
}

.cyberCell {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid var(--c-border);
  padding: 10px 15px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.flexGrow { flex-grow: 1; }

/* Efecto de Nodo de Estado */
.statusNode {
  width: 8px; height: 8px;
  background: var(--c-neon);
  box-shadow: 0 0 10px var(--c-neon);
  display: inline-block;
  margin-right: 10px;
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
}

.cyberText {
  color: var(--c-neon);
  font-size: 0.8rem;
  text-shadow: 0 0 5px var(--c-neon);
}

/* Barra de Energía Cyber */
.dataHeader {
  display: flex;
  justify-content: space-between;
  margin-bottom: 6px;
}

.miniLabel { font-size: 0.6rem; color: #666; letter-spacing: 1px; }
.valueText { font-size: 0.8rem; color: #fff; }

.energyTrack {
  height: 6px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 1px;
}

.energyFill {
  height: 100%;
  background: var(--c-neon);
  box-shadow: 0 0 15px var(--c-neon);
  position: relative;
  transition: width 0.3s ease;
}

.scanline {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
  width: 20%;
  animation: scan 2s infinite linear;
}

@keyframes scan {
  from { left: -20%; }
  to { left: 120%; }
}

/* Reloj de Precisión */
.clockCell {
  background: rgba(0, 243, 255, 0.1);
  min-width: 110px;
  border-left: 3px solid var(--c-neon);
}

.timeWrapper {
  display: flex;
  align-items: baseline;
  justify-content: center;
  color: var(--c-neon);
}

.timeMain { font-size: 1.2rem; font-weight: bold; }
.timeSec { font-size: 0.7rem; margin-left: 4px; opacity: 0.7; }

/* Detalles estéticos */
.cornerTrim {
  position: absolute;
  top: -1px; right: -1px;
  width: 10px; height: 10px;
  background: var(--c-dark);
  clip-path: polygon(100% 0, 0 0, 100% 100%);
  border-left: 1px solid var(--c-border);
  border-bottom: 1px solid var(--c-border);
}

.decoLines {
  position: absolute;
  bottom: 2px; left: 0; right: 0;
  height: 2px;
  background: repeating-linear-gradient(90deg, var(--c-neon) 0 2px, transparent 2px 4px);
  opacity: 0.3;
}
</style>