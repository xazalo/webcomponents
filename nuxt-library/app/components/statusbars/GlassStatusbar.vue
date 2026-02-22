<template>
  <div :class="$style.glassWrapper">
    <div :class="$style.glassBar" :style="glassStyles">
      
      <div :class="$style.section">
        <div :class="$style.glassDot"></div>
        <span :class="$style.brandText">OS_VISION</span>
      </div>

      <div :class="$style.separator"></div>

      <div :class="[$style.section, $style.flexGrow]">
        <div :class="$style.statsGroup">
          <span :class="$style.labelText">CPU_LOAD</span>
          <div :class="$style.track">
            <div :class="$style.progress" :style="{ width: load + '%' }"></div>
          </div>
          <span :class="$style.valueText">{{ load }}%</span>
        </div>
      </div>

      <div :class="[$style.section, $style.timeSection]">
        <span :class="$style.time">{{ currentTime }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';

const props = defineProps<{ color?: string }>();
const load = ref(42);
const currentTime = ref('00:00');

const updateTime = () => {
  const now = new Date();
  currentTime.value = now.toLocaleTimeString('en-US', { 
    hour: '2-digit', minute: '2-digit', hour12: false 
  });
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
});

const glassStyles = computed(() => ({
  '--g-accent': props.color || 'rgba(255, 255, 255, 0.5)',
  '--g-tint': 'rgba(255, 255, 255, 0.1)',
}));
</script>

<style module>
.glassWrapper {
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); /* Fondo para notar el efecto */
  border-radius: 24px;
}

.glassBar {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 6px 6px 6px 18px;
  background: var(--g-tint);
  
  /* EL CORE DEL GLASSMORPHISM */
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
  
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 100px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
  width: fit-content;
  font-family: 'Inter', system-ui, sans-serif;
}

.section {
  display: flex;
  align-items: center;
  gap: 10px;
}

.flexGrow { flex-grow: 1; min-width: 220px; }

.glassDot {
  width: 8px;
  height: 8px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 10px #fff;
}

.brandText {
  color: #fff;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 1px;
  opacity: 0.9;
}

.separator {
  width: 1px;
  height: 20px;
  background: rgba(255, 255, 255, 0.2);
}

.statsGroup {
  display: flex;
  align-items: center;
  gap: 12px;
  width: 100%;
}

.labelText {
  font-size: 0.65rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.6);
}

.track {
  height: 4px;
  flex-grow: 1;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  overflow: hidden;
}

.progress {
  height: 100%;
  background: #fff;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
  transition: width 1s ease;
}

.valueText {
  font-size: 0.75rem;
  font-weight: 600;
  color: #fff;
  min-width: 35px;
}

.timeSection {
  background: rgba(255, 255, 255, 0.2);
  padding: 8px 20px;
  border-radius: 100px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.time {
  color: #fff;
  font-size: 0.85rem;
  font-weight: 800;
  letter-spacing: 0.5px;
}
</style>