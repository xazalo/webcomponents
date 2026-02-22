<template>
  <div :class="$style.bentoBar" :style="bentoStyles">
    <div :class="$style.cell">
      <div :class="$style.statusIndicator"></div>
      <span :class="$style.textBold">SYSTEM_OS</span>
      <span :class="$style.version">v2.0.4</span>
    </div>

    <div :class="[$style.cell, $style.flexGrow]">
      <span :class="$style.label">UPTIME</span>
      <div :class="$style.progressTrack">
        <div :class="$style.progressBar" :style="{ width: uptime + '%' }"></div>
      </div>
      <span :class="$style.value">{{ uptime }}%</span>
    </div>

    <div :class="$style.cell">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
        <path d="m5 12 5 5L20 7"/>
      </svg>
      <span :class="$style.value">{{ latency }}ms</span>
    </div>

    <div :class="[$style.cell, $style.timeCell]">
      <span :class="$style.time">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const props = defineProps<{ color?: string }>();

const uptime = ref(84);
const latency = ref(24);
const currentTime = ref('');

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
  setInterval(updateTime, 1000);
});

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoBar {
  display: flex;
  gap: 8px;
  padding: 8px;
  background: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: 16px;
  font-family: 'Inter', system-ui, sans-serif;
  width: fit-content;
}

.cell {
  background: #ffffff;
  padding: 6px 12px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
  border: 1px solid rgba(0, 0, 0, 0.03);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
}

.flexGrow { flex-grow: 1; min-width: 200px; }

.statusIndicator {
  width: 8px;
  height: 8px;
  background: #10b981;
  border-radius: 50%;
  box-shadow: 0 0 8px rgba(16, 185, 129, 0.4);
}

.textBold {
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.05em;
  color: #111827;
}

.version {
  font-size: 0.7rem;
  font-weight: 500;
  color: #9ca3af;
}

.label {
  font-size: 0.65rem;
  font-weight: 700;
  color: #6b7280;
  text-transform: uppercase;
}

.progressTrack {
  height: 6px;
  flex-grow: 1;
  background: #f3f4f6;
  border-radius: 3px;
  overflow: hidden;
}

.progressBar {
  height: 100%;
  background: var(--b-accent);
  border-radius: 3px;
  transition: width 0.5s ease;
}

.value {
  font-size: 0.75rem;
  font-weight: 700;
  color: #374151;
  font-variant-numeric: tabular-nums;
}

.timeCell {
  background: var(--b-accent);
  color: #ffffff;
}

.time {
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: -0.02em;
}
</style>