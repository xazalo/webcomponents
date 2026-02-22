<template>
  <div :class="$style.neuContainer" :style="neuStyles">
    <div :class="$style.neuBar">
      
      <div :class="[$style.section, $style.insetPanel]">
        <div :class="$style.statusLight"></div>
        <span :class="$style.brandText">SYSTEM_LINK</span>
      </div>

      <div :class="[$style.section, $style.flexGrow]">
        <span :class="$style.labelText">CPU</span>
        <div :class="$style.track">
          <div :class="$style.progress" :style="{ width: load + '%' }"></div>
        </div>
        <span :class="$style.valueText">{{ load }}%</span>
      </div>

      <div :class="[$style.section, $style.timePanel]">
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

const neuStyles = computed(() => ({
  '--n-bg': '#e0e5ec',
  '--n-accent': props.color || '#4f46e5',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.8)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
}));
</script>

<style module>
.neuContainer {
  padding: 40px;
  background: var(--n-bg);
  border-radius: 30px;
  display: flex;
  justify-content: center;
}

.neuBar {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 12px 25px;
  background: var(--n-bg);
  border-radius: 20px;
  
  /* Elevación principal de la barra */
  box-shadow: 
    9px 9px 16px var(--n-shadow-dark), 
    -9px -9px 16px var(--n-shadow-light);
  
  width: fit-content;
  font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

.section {
  display: flex;
  align-items: center;
  gap: 12px;
}

.flexGrow { flex-grow: 1; min-width: 200px; }

/* Panel Inset (Hundido) */
.insetPanel {
  padding: 8px 15px;
  border-radius: 12px;
  box-shadow: 
    inset 3px 3px 6px var(--n-shadow-dark), 
    inset -3px -3px 6px var(--n-shadow-light);
}

.statusLight {
  width: 8px;
  height: 8px;
  background: #4ade80;
  border-radius: 50%;
  box-shadow: 0 0 8px rgba(74, 222, 128, 0.5);
}

.brandText {
  color: #7a8ba9;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 1px;
}

.labelText {
  font-size: 0.7rem;
  font-weight: 800;
  color: #9baacf;
}

/* Track de carga hundido */
.track {
  height: 10px;
  flex-grow: 1;
  background: var(--n-bg);
  border-radius: 10px;
  box-shadow: 
    inset 2px 2px 5px var(--n-shadow-dark), 
    inset -2px -2px 5px var(--n-shadow-light);
  overflow: hidden;
  padding: 2px;
}

.progress {
  height: 100%;
  background: var(--n-accent);
  border-radius: 10px;
  box-shadow: 2px 0 4px rgba(0,0,0,0.1);
  transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.valueText {
  font-size: 0.8rem;
  font-weight: 700;
  color: #7a8ba9;
  min-width: 35px;
}

/* Panel de Tiempo con elevación extra */
.timePanel {
  padding: 8px 20px;
  border-radius: 12px;
  background: var(--n-bg);
  box-shadow: 
    4px 4px 8px var(--n-shadow-dark), 
    -4px -4px 8px var(--n-shadow-light);
}

.time {
  color: var(--n-accent);
  font-size: 0.9rem;
  font-weight: 800;
}
</style>