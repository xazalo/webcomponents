<template>
  <div v-bind="$attrs" :class="$style.glassRoot" :style="glassStyles">
    <div :class="$style.glassBar">
      
      <div :class="$style.section">
        <div :class="[$style.glassDot, animated && $style.pulseAnim]"></div>
        <span :class="$style.brandText">{{ brandName }}</span>
      </div>

      <div :class="$style.separator"></div>

      <div v-if="showStats" :class="[$style.section, $style.flexGrow]">
        <div :class="$style.statsGroup">
          <span :class="$style.labelText">{{ statsLabel }}</span>
          <div :class="$style.track">
            <div :class="$style.progress" :style="{ width: loadValue + '%' }"></div>
          </div>
          <span :class="$style.valueText">{{ loadValue }}%</span>
        </div>
      </div>

      <div :class="[$style.section, $style.timeSection]">
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
  statsLabel?: string;
  loadValue?: number;
  color?: string;       /* Color del acento y progreso */
  // Glass Tuning
  blur?: number;        /* Nivel de desenfoque (px) */
  opacity?: number;     /* Opacidad del fondo (0-1) */
  saturation?: number;  /* Saturación de colores de fondo (%) */
  animated?: boolean;   /* ¿Pulso en el dot y transiciones? */
  showStats?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  brandName: 'OS_VISION',
  statsLabel: 'CPU_LOAD',
  loadValue: 42,
  color: '#ffffff',
  blur: 12,
  opacity: 0.15,
  saturation: 180,
  animated: true,
  showStats: true
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

const glassStyles = computed(() => ({
  '--g-accent': props.color,
  '--g-blur': `${props.blur}px`,
  '--g-opacity': props.opacity,
  '--g-sat': `${props.saturation}%`,
  '--g-border': `rgba(255, 255, 255, ${props.opacity + 0.1})`,
}));
</script>

<style module>
.glassRoot {
  width: fit-content;
  padding: 10px;
}

.glassBar {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 6px 6px 6px 18px;
  background: rgba(255, 255, 255, var(--g-opacity));
  
  /* Renderizado de Cristal */
  backdrop-filter: blur(var(--g-blur)) saturate(var(--g-sat));
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(var(--g-sat));
  
  border: 1px solid var(--g-border);
  border-radius: 100px;
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.1);
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
}

.section { display: flex; align-items: center; gap: 10px; }
.flexGrow { flex-grow: 1; min-width: 200px; }

.glassDot {
  width: 8px; height: 8px;
  background: var(--g-accent);
  border-radius: 50%;
  box-shadow: 0 0 10px var(--g-accent);
}

.pulseAnim { animation: pulse 2s infinite; }

@keyframes pulse {
  0% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.5); opacity: 0.5; box-shadow: 0 0 20px var(--g-accent); }
  100% { transform: scale(1); opacity: 1; }
}

.brandText {
  color: #fff; font-size: 0.7rem; font-weight: 800;
  letter-spacing: 0.1em; text-transform: uppercase;
}

.separator {
  width: 1px; height: 16px;
  background: var(--g-border);
}

.statsGroup {
  display: flex; align-items: center;
  gap: 12px; width: 100%;
}

.labelText {
  font-size: 0.6rem; font-weight: 700;
  color: rgba(255, 255, 255, 0.5); letter-spacing: 0.05em;
}

.track {
  height: 4px; flex-grow: 1;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px; overflow: hidden;
}

.progress {
  height: 100%; background: var(--g-accent);
  box-shadow: 0 0 12px var(--g-accent);
  transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
}

.valueText {
  font-size: 0.75rem; font-weight: 700;
  color: #fff; min-width: 35px;
}

.timeSection {
  background: rgba(255, 255, 255, 0.15);
  padding: 8px 22px;
  border-radius: 100px;
  border: 1px solid var(--g-border);
  margin-left: 10px;
}

.time {
  color: #fff; font-size: 0.85rem;
  font-weight: 850; font-variant-numeric: tabular-nums;
}
</style>