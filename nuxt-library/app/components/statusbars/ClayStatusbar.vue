<template>
  <div v-bind="$attrs" :class="$style.clayRoot" :style="clayStyles">
    <div :class="$style.cell">
      <div :class="$style.indicator">
        <div :class="$style.innerDot"></div>
      </div>
      <span :class="$style.label">{{ label }}</span>
    </div>

    <div v-if="showProgress" :class="[$style.cell, $style.flexGrow]">
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: progressValue + '%' }"></div>
      </div>
      <span :class="$style.value">{{ progressValue }}%</span>
    </div>

    <div :class="$style.cell">
      <div :class="$style.signalGroup">
        <div 
          v-for="i in 3" 
          :key="i" 
          :class="$style.clayDot" 
          :style="{ 
            backgroundColor: i <= signalLevel ? 'var(--c-accent)' : '#cbd5e1',
            opacity: i <= signalLevel ? 1 : 0.4
          }"
        ></div>
      </div>
    </div>

    <div :class="[$style.cell, $style.clockCell]">
      <span :class="$style.timeText">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';

interface Props {
  label?: string;
  progressValue?: number;
  signalLevel?: number;
  color?: string;       /* Color de acento (ej: #38bdf8) */
  // Clay Tuning
  depth?: number;       /* Intensidad de la sombra interna (0-20) */
  radius?: string;      /* Redondez de las celdas */
  spread?: number;      /* Difusión de la sombra exterior */
  showProgress?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  label: 'SYSTEM',
  progressValue: 62,
  signalLevel: 2,
  color: '#38bdf8',
  depth: 8,
  radius: '22px',
  spread: 16,
  showProgress: true
});

const currentTime = ref('');
let timer: any = null;

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-US', { 
    hour: '2-digit', minute: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});

onUnmounted(() => clearInterval(timer));

const clayStyles = computed(() => {
  const d = props.depth;
  const s = props.spread;
  return {
    '--c-accent': props.color,
    '--c-radius': props.radius,
    '--c-bg': '#ffffff',
    // Sombra proyectada suave
    '--c-outer-shadow': `0 ${s/2}px ${s}px rgba(0, 0, 0, 0.06)`,
    // Sombras internas para el efecto "arcilla"
    '--c-inner-dark': `inset -${d}px -${d}px ${d*2}px rgba(0, 0, 0, 0.08)`,
    '--c-inner-light': `inset ${d}px ${d}px ${d*2}px rgba(255, 255, 255, 1)`,
    // Sombra interna específica para el color de acento
    '--c-accent-dark': `inset -${d}px -${d}px ${d*2}px rgba(0, 0, 0, 0.15)`,
    '--c-accent-light': `inset ${d}px ${d}px ${d*2}px rgba(255, 255, 255, 0.4)`,
  };
});
</script>

<style module>
.clayRoot {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px;
  background: transparent;
  width: fit-content;
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: var(--c-bg);
  padding: 10px 18px;
  border-radius: var(--c-radius);
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(255, 255, 255, 0.4);
  
  /* El efecto Claymorphism */
  box-shadow: 
    var(--c-outer-shadow),
    var(--c-inner-dark),
    var(--c-inner-light);
    
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.cell:hover {
  transform: translateY(-2px);
  box-shadow: 
    0 12px 20px rgba(0, 0, 0, 0.08),
    var(--c-inner-dark),
    var(--c-inner-light);
}

.flexGrow { flex-grow: 1; min-width: 180px; }

.indicator {
  width: 14px; height: 14px;
  background: #4ade80;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 
    inset -2px -2px 4px rgba(0,0,0,0.15), 
    inset 2px 2px 4px rgba(255,255,255,0.5);
}

.innerDot {
  width: 4px; height: 4px;
  background: white;
  border-radius: 50%;
  filter: blur(0.5px);
}

.label {
  font-size: 0.7rem;
  font-weight: 800;
  color: #94a3b8;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.track {
  height: 12px;
  flex-grow: 1;
  background: #f1f5f9;
  border-radius: 20px;
  box-shadow: inset 2px 2px 6px rgba(0,0,0,0.08);
  overflow: hidden;
  position: relative;
}

.fill {
  height: 100%;
  background: var(--c-accent);
  border-radius: 20px;
  transition: width 1s cubic-bezier(0.65, 0, 0.35, 1);
  box-shadow: inset 0 4px 6px rgba(255,255,255,0.3);
}

.value {
  font-size: 0.85rem;
  font-weight: 900;
  color: var(--c-accent);
  font-variant-numeric: tabular-nums;
}

.signalGroup { display: flex; gap: 6px; }
.clayDot {
  width: 10px; height: 10px;
  border-radius: 50%;
  box-shadow: 
    inset -2px -2px 4px rgba(0,0,0,0.1), 
    inset 2px 2px 3px rgba(255,255,255,0.8);
  transition: all 0.3s ease;
}

.clockCell {
  background: var(--c-accent);
  color: white;
  box-shadow: 
    var(--c-outer-shadow),
    var(--c-accent-dark),
    var(--c-accent-light);
}

.timeText {
  font-size: 0.95rem;
  font-weight: 900;
  letter-spacing: 0.5px;
}
</style>