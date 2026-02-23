<template>
  <div v-bind="$attrs" :class="$style.bentoRoot" :style="bentoStyles">
    <div :class="$style.cell">
      <div 
        :class="$style.statusIndicator" 
        :style="{ background: statusColor, boxShadow: `0 0 8px ${statusColor}66` }"
      ></div>
      <div :class="$style.stack">
        <span :class="$style.textBold">{{ systemName }}</span>
        <span :class="$style.version">{{ versionText }}</span>
      </div>
    </div>

    <div v-if="showProgress" :class="[$style.cell, $style.flexGrow]">
      <span :class="$style.label">{{ progressLabel }}</span>
      <div :class="$style.progressTrack">
        <div :class="$style.progressBar" :style="{ width: progressValue + '%' }"></div>
      </div>
      <span :class="$style.value">{{ progressValue }}%</span>
    </div>

    <div :class="$style.cell">
      <component :is="secondaryIcon" v-if="secondaryIcon" :class="$style.icon" />
      <div :class="$style.stack">
        <span :class="$style.microLabel">{{ secondaryLabel }}</span>
        <span :class="$style.value">{{ secondaryValue }}</span>
      </div>
    </div>

    <div :class="[$style.cell, $style.timeCell]">
      <span :class="$style.time">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';

// Permitir pasar atributos al contenedor principal sin herencia automática en los hijos
defineOptions({ inheritAttrs: false });

interface Props {
  // Datos del Sistema
  systemName?: string;
  versionText?: string;
  statusColor?: string;
  
  // Datos de Progreso
  progressLabel?: string;
  progressValue?: number;
  showProgress?: boolean;

  // Datos Secundarios
  secondaryLabel?: string;
  secondaryValue?: string | number;
  secondaryIcon?: any; // Puede ser un componente de icono (Lucide, etc)

  // Estética
  accentColor?: string;
  borderRadius?: string;
  glassOpacity?: number;
}

const props = withDefaults(defineProps<Props>(), {
  systemName: 'SYSTEM_OS',
  versionText: 'v2.0.4',
  statusColor: '#10b981',
  progressLabel: 'UPTIME',
  progressValue: 84,
  showProgress: true,
  secondaryLabel: 'LATENCY',
  secondaryValue: '24ms',
  accentColor: '#4f46e5',
  borderRadius: '16px',
  glassOpacity: 0.6
});

const currentTime = ref('');
let timer: any = null;

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-US', { 
    hour12: false, 
    hour: '2-digit', 
    minute: '2-digit' 
  });
};

onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});

onUnmounted(() => {
  if (timer) clearInterval(timer);
});

const bentoStyles = computed(() => ({
  '--b-accent': props.accentColor,
  '--b-radius': props.borderRadius,
  '--b-glass': `rgba(255, 255, 255, ${props.glassOpacity})`,
}));
</script>

<style module>
.bentoRoot {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: var(--b-glass);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: var(--b-radius);
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  width: fit-content;
  box-sizing: border-box;
}

.cell {
  background: #ffffff;
  padding: 8px 14px;
  border-radius: calc(var(--b-radius) - 6px);
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(0, 0, 0, 0.03);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
}

.flexGrow { flex-grow: 1; min-width: 220px; }

.stack {
  display: flex;
  flex-direction: column;
  line-height: 1.2;
}

.statusIndicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  flex-shrink: 0;
}

.textBold {
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.02em;
  color: #111827;
}

.version {
  font-size: 0.65rem;
  font-weight: 600;
  color: #9ca3af;
}

.label {
  font-size: 0.65rem;
  font-weight: 700;
  color: #6b7280;
  text-transform: uppercase;
  margin-right: 4px;
}

.microLabel {
  font-size: 0.6rem;
  font-weight: 700;
  color: #9ca3af;
  text-transform: uppercase;
}

.progressTrack {
  height: 8px;
  flex-grow: 1;
  background: #f3f4f6;
  border-radius: 4px;
  overflow: hidden;
  position: relative;
}

.progressBar {
  height: 100%;
  background: var(--b-accent);
  border-radius: 4px;
  transition: width 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.value {
  font-size: 0.8rem;
  font-weight: 700;
  color: #1f2937;
  font-variant-numeric: tabular-nums;
}

.icon {
  color: var(--b-accent);
  flex-shrink: 0;
}

.timeCell {
  background: var(--b-accent);
  color: #ffffff;
  padding: 8px 20px;
}

.time {
  font-size: 0.9rem;
  font-weight: 900;
  letter-spacing: -0.02em;
}

/* Animación sutil de entrada para las celdas */
.cell {
  animation: slideIn 0.4s ease-out backwards;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateY(4px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>