<template>
  <div :class="$style.clayBar" :style="clayStyles">
    <div :class="$style.clayCell">
      <div :class="$style.indicator">
        <div :class="$style.innerDot"></div>
      </div>
      <span :class="$style.label">SYSTEM</span>
    </div>

    <div :class="[$style.clayCell, $style.flexGrow]">
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: load + '%' }"></div>
      </div>
      <span :class="$style.value">{{ load }}%</span>
    </div>

    <div :class="$style.clayCell">
      <div :class="$style.signalGroup">
        <div v-for="i in 3" :key="i" :class="$style.clayDot" :style="{ opacity: i < 3 ? 1 : 0.3 }"></div>
      </div>
    </div>

    <div :class="[$style.clayCell, $style.clockCell]">
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
  currentTime.value = now.toLocaleTimeString('en-US', { 
    hour: '2-digit', 
    minute: '2-digit',
    hour12: false 
  });
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
});

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#38bdf8', // Azul cielo por defecto
  '--c-bg': '#ffffff',
  '--c-shadow-inner': 'rgba(0, 0, 0, 0.08)',
  '--c-highlight': 'rgba(255, 255, 255, 1)'
}));
</script>

<style module>
.clayBar {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 12px 24px;
  background: #f0f4f8; /* Color de fondo para que resalte el blanco */
  border-radius: 30px;
  width: fit-content;
}

.clayCell {
  background: var(--c-bg);
  padding: 10px 18px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  
  /* EL SECRETO DEL CLAYMORPHISM: Sombras combinadas */
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.05),              /* Sombra proyectada */
    inset -6px -6px 12px var(--c-shadow-inner),    /* Profundidad inferior */
    inset 6px 6px 12px var(--c-highlight);         /* Brillo superior */
    
  transition: transform 0.2s ease;
}

.flexGrow { flex-grow: 1; min-width: 180px; }

.indicator {
  width: 14px;
  height: 14px;
  background: #4ade80;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: inset 2px 2px 4px rgba(255,255,255,0.5), inset -2px -2px 4px rgba(0,0,0,0.1);
}

.innerDot {
  width: 4px;
  height: 4px;
  background: white;
  border-radius: 50%;
  filter: blur(1px);
}

.label {
  font-size: 0.75rem;
  font-weight: 800;
  color: #94a3b8;
  letter-spacing: 0.5px;
}

.track {
  height: 12px;
  flex-grow: 1;
  background: #f1f5f9;
  border-radius: 10px;
  box-shadow: inset 2px 2px 5px rgba(0,0,0,0.05);
  overflow: hidden;
}

.fill {
  height: 100%;
  background: var(--c-accent);
  border-radius: 10px;
  transition: width 1s ease-in-out;
  /* Brillo en la barra de carga */
  box-shadow: inset 0 4px 4px rgba(255,255,255,0.3);
}

.value {
  font-size: 0.8rem;
  font-weight: 900;
  color: var(--c-accent);
}

.signalGroup { display: flex; gap: 4px; }
.clayDot {
  width: 8px;
  height: 8px;
  background: #cbd5e1;
  border-radius: 50%;
  box-shadow: inset 1px 1px 2px rgba(0,0,0,0.1), 1px 1px 2px white;
}

.clockCell {
  background: var(--c-accent);
  color: white;
  /* Sombra específica para el color de acento */
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.1),
    inset -6px -6px 12px rgba(0, 0, 0, 0.2),
    inset 6px 6px 12px rgba(255, 255, 255, 0.4);
}

.timeText {
  font-size: 1rem;
  font-weight: 900;
}
</style>