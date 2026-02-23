<template>
  <div v-bind="$attrs" :class="$style.neuRoot" :style="neuStyles">
    <div :class="$style.neuBar">
      
      <div :class="[$style.section, $style.insetPanel]">
        <div :class="[$style.statusLight, animated && $style.glowAnim]"></div>
        <span :class="$style.brandText">{{ brandName }}</span>
      </div>

      <div v-if="showProgress" :class="[$style.section, $style.flexGrow]">
        <span :class="$style.labelText">{{ progressLabel }}</span>
        <div :class="$style.track">
          <div :class="$style.progress" :style="{ width: loadValue + '%' }"></div>
        </div>
        <span :class="$style.valueText">{{ loadValue }}%</span>
      </div>

      <div :class="[$style.section, $style.timePanel]">
        <span :class="$style.time">{{ currentTime }}</span>
      </div>
      
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  brandName?: string;
  progressLabel?: string;
  loadValue?: number;
  color?: string;       /* Color de acento */
  // Neu Tuning
  baseBg?: string;      /* Color de fondo (Clave para el efecto) */
  distance?: number;    /* Distancia de la sombra (5-15) */
  intensity?: number;   /* Opacidad de las sombras (0-1) */
  blur?: number;        /* Difuminado de la sombra */
  radius?: string;      /* Redondez */
  animated?: boolean;
  showProgress?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  brandName: 'SYSTEM_LINK',
  progressLabel: 'CPU',
  loadValue: 42,
  color: '#4f46e5',
  baseBg: '#e0e5ec',
  distance: 8,
  intensity: 0.2,
  blur: 16,
  radius: '20px',
  animated: true,
  showProgress: true
});

const currentTime = ref('00:00');
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

const neuStyles = computed(() => {
  const d = props.distance;
  const b = props.blur;
  const i = props.intensity;
  
  return {
    '--n-bg': props.baseBg,
    '--n-accent': props.color,
    '--n-radius': props.radius,
    // Sombras calculadas
    '--n-shadow-dark': `rgba(0, 0, 0, ${i})`,
    '--n-shadow-light': `rgba(255, 255, 255, ${i + 0.6})`,
    '--n-dist': `${d}px`,
    '--n-blur': `${b}px`,
  };
});
</script>

<style module>
.neuRoot {
  width: fit-content;
  padding: 20px;
  background: var(--n-bg);
  border-radius: calc(var(--n-radius) * 1.5);
}

.neuBar {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 10px 20px;
  background: var(--n-bg);
  border-radius: var(--n-radius);
  
  /* Elevación principal */
  box-shadow: 
    var(--n-dist) var(--n-dist) var(--n-blur) var(--n-shadow-dark), 
    calc(var(--n-dist) * -1) calc(var(--n-dist) * -1) var(--n-blur) var(--n-shadow-light);
  
  font-family: 'Inter', system-ui, sans-serif;
}

.section { display: flex; align-items: center; gap: 12px; }
.flexGrow { flex-grow: 1; min-width: 220px; }

/* Efecto Inset (Hundido) */
.insetPanel {
  padding: 8px 15px;
  border-radius: calc(var(--n-radius) * 0.6);
  box-shadow: 
    inset 2px 2px 5px var(--n-shadow-dark), 
    inset -2px -2px 5px var(--n-shadow-light);
}

.statusLight {
  width: 8px; height: 8px;
  background: #4ade80;
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(74, 222, 128, 0.4);
}

.glowAnim { animation: glow 2s ease-in-out infinite; }
@keyframes glow {
  0%, 100% { opacity: 1; filter: brightness(1); }
  50% { opacity: 0.6; filter: brightness(1.2); }
}

.brandText {
  color: #7a8ba9; font-size: 0.65rem;
  font-weight: 800; letter-spacing: 0.1em;
}

.labelText {
  font-size: 0.7rem; font-weight: 800;
  color: #9baacf; margin-right: 4px;
}

/* Track Hundido con precisión */
.track {
  height: 12px; flex-grow: 1;
  background: var(--n-bg);
  border-radius: 20px;
  box-shadow: 
    inset 2px 2px 4px var(--n-shadow-dark), 
    inset -2px -2px 4px var(--n-shadow-light);
  overflow: hidden;
  padding: 3px;
}

.progress {
  height: 100%;
  background: var(--n-accent);
  border-radius: 20px;
  transition: width 1s cubic-bezier(0.22, 1, 0.36, 1);
  box-shadow: 1px 1px 2px rgba(0,0,0,0.2);
}

.valueText {
  font-size: 0.8rem; font-weight: 800;
  color: #7a8ba9; min-width: 40px;
  font-variant-numeric: tabular-nums;
}

/* Panel Elevado (Extruded) */
.timePanel {
  padding: 8px 18px;
  border-radius: calc(var(--n-radius) * 0.6);
  background: var(--n-bg);
  box-shadow: 
    calc(var(--n-dist) / 2) calc(var(--n-dist) / 2) var(--n-blur) var(--n-shadow-dark), 
    calc(var(--n-dist) / -2) calc(var(--n-dist) / -2) var(--n-blur) var(--n-shadow-light);
}

.time {
  color: var(--n-accent);
  font-size: 0.9rem; font-weight: 900;
  letter-spacing: -0.5px;
}
</style>