<template>
  <div :class="$style.brutalBar" :style="brutalStyles">
    <div :class="$style.panel">
      <div :class="$style.dot"></div>
      <span :class="$style.title">SYS.MODE</span>
      <span :class="$style.tag">ACTIVE</span>
    </div>

    <div :class="[$style.panel, $style.flexGrow]">
      <span :class="$style.label">CORE_LOAD</span>
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: load + '%' }"></div>
      </div>
      <span :class="$style.number">{{ load }}%</span>
    </div>

    <div :class="$style.panel">
      <span :class="$style.label">SYNC</span>
      <div :class="$style.signal">
        <div v-for="i in 4" :key="i" :class="[$style.bar, i < 4 && $style.barActive]"></div>
      </div>
    </div>

    <div :class="[$style.panel, $style.clockPanel]">
      <span :class="$style.timeText">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const props = defineProps<{ color?: string }>();
const load = ref(62);
const currentTime = ref('');

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-GB', { 
    hour: '2-digit', 
    minute: '2-digit',
    second: '2-digit'
  });
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
});

const brutalStyles = computed(() => ({
  '--n-accent': props.color || '#ffff00', // Amarillo "caution" por defecto
}));
</script>

<style module>
.brutalBar {
  display: flex;
  background: #000;
  padding: 4px;
  gap: 4px;
  width: 100%;
  box-sizing: border-box;
  font-family: 'Arial Black', sans-serif;
  border: 4px solid #000;
}

.panel {
  background: #fff;
  border: 2px solid #000;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 4px 4px 0px #000;
}

.flexGrow { flex-grow: 1; }

.dot {
  width: 12px;
  height: 12px;
  background: #ff0000;
  border: 2px solid #000;
  animation: blink 1s steps(2) infinite;
}

.title {
  font-size: 0.8rem;
  font-weight: 900;
  letter-spacing: -0.05em;
}

.tag {
  background: #000;
  color: #fff;
  padding: 2px 6px;
  font-size: 0.65rem;
  font-weight: 900;
}

.label {
  font-size: 0.7rem;
  font-weight: 900;
  text-transform: uppercase;
}

.track {
  height: 14px;
  flex-grow: 1;
  background: #fff;
  border: 2px solid #000;
  position: relative;
}

.fill {
  height: 100%;
  background: var(--n-accent);
  border-right: 2px solid #000;
}

.number {
  font-size: 0.9rem;
  font-weight: 900;
}

.signal {
  display: flex;
  align-items: flex-end;
  gap: 2px;
  height: 12px;
}

.bar {
  width: 4px;
  background: #e0e0e0;
  border: 1px solid #000;
}
.bar:nth-child(1) { height: 25%; }
.bar:nth-child(2) { height: 50%; }
.bar:nth-child(3) { height: 75%; }
.bar:nth-child(4) { height: 100%; }

.barActive { background: #00ff00; }

.clockPanel {
  background: var(--n-accent);
  min-width: 120px;
  justify-content: center;
}

.timeText {
  font-size: 1.1rem;
  font-weight: 900;
  color: #000;
}

@keyframes blink {
  from { opacity: 1; }
  to { opacity: 0; }
}
</style>