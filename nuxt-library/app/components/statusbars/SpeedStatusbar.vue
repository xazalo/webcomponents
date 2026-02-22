<template>
  <div :class="$style.speedContainer" :style="speedStyles">
    <div :class="$style.speedBar">
      
      <div :class="$style.gearSection">
        <div :class="$style.skewBox">
          <span :class="$style.gearLetter">R</span>
        </div>
        <div :class="[$style.skewBox, $style.activeGear]">
          <span :class="$style.gearLetter">D</span>
        </div>
      </div>

      <div :class="[$style.section, $style.flexGrow]">
        <div :class="$style.gaugeLabel">
          <span :class="$style.italicText">CORE_TEMP</span>
          <span :class="$style.valueText">{{ load }}%</span>
        </div>
        <div :class="$style.tachoTrack">
          <div 
            v-for="i in 20" 
            :key="i" 
            :class="[$style.tachoStep, (i/20 * 100) <= load && $style.tachoStepActive]"
          ></div>
        </div>
      </div>

      <div :class="$style.clockSection">
        <div :class="$style.timeWrapper">
          <span :class="$style.timeMain">{{ currentTime }}</span>
          <span :class="$style.unit">GMT</span>
        </div>
        <div :class="$style.nitroLine"></div>
      </div>
      
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const props = defineProps<{ color?: string }>();
const load = ref(74);
const currentTime = ref('00:00');

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-GB', { 
    hour: '2-digit', minute: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
  
  // Simulación de fluctuación de carga "racing"
  setInterval(() => {
    load.value = Math.floor(Math.random() * (85 - 70 + 1)) + 70;
  }, 2000);
});

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#ff3e00', // Rojo Racing / Naranja Fuego
  '--s-bg': '#050505',
}));
</script>

<style module>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&display=swap');

.speedContainer {
  padding: 20px;
  background: #111;
  display: flex;
  justify-content: center;
}

.speedBar {
  display: flex;
  align-items: center;
  gap: 15px;
  background: var(--s-bg);
  padding: 5px 5px 5px 15px;
  border-left: 5px solid var(--s-accent);
  transform: skewX(-10deg); /* Inclinación general de velocidad */
  box-shadow: 10px 10px 0px rgba(0,0,0,0.5);
  font-family: 'Orbitron', sans-serif;
}

.section { display: flex; flex-direction: column; gap: 4px; }
.flexGrow { flex-grow: 1; min-width: 250px; }

/* Selector de Marchas */
.gearSection { display: flex; gap: 4px; }
.skewBox {
  background: #222;
  padding: 4px 10px;
  border: 1px solid #333;
}
.activeGear {
  background: var(--s-accent);
  color: #000;
  box-shadow: 0 0 15px var(--s-accent);
}
.gearLetter {
  display: block;
  transform: skewX(10deg); /* Corregir inclinación del texto */
  font-size: 0.9rem;
  font-weight: 900;
}

/* Tacómetro de pasos */
.gaugeLabel {
  display: flex;
  justify-content: space-between;
  transform: skewX(10deg);
}
.italicText { font-size: 0.6rem; color: #666; font-style: italic; }
.valueText { font-size: 0.8rem; color: #eee; }

.tachoTrack {
  display: flex;
  gap: 3px;
  height: 12px;
}
.tachoStep {
  flex: 1;
  background: #222;
  transform: skewX(-5deg);
  transition: all 0.2s ease;
}
.tachoStepActive {
  background: var(--s-accent);
  box-shadow: 0 0 8px var(--s-accent);
}

/* Reloj de alta velocidad */
.clockSection {
  background: #1a1a1a;
  padding: 10px 25px;
  position: relative;
  overflow: hidden;
  border-right: 2px solid var(--s-accent);
}
.timeWrapper {
  transform: skewX(10deg);
  display: flex;
  align-items: baseline;
  gap: 4px;
}
.timeMain { font-size: 1.4rem; font-weight: 900; color: #fff; }
.unit { font-size: 0.6rem; color: var(--s-accent); }

.nitroLine {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 3px;
  width: 100%;
  background: linear-gradient(90deg, transparent, var(--s-accent));
  animation: nitroFlow 1s infinite linear;
}

@keyframes nitroFlow {
  from { transform: translateX(-100%); }
  to { transform: translateX(100%); }
}
</style>