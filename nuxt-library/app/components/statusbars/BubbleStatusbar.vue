<template>
  <div :class="$style.bubbleBar" :style="bubbleStyles">
    <div :class="$style.bubble">
      <div :class="$style.pulseContainer">
        <div :class="$style.dot"></div>
        <div :class="$style.pulse"></div>
      </div>
      <span :class="$style.text">System Live</span>
    </div>

    <div :class="[$style.bubble, $style.flexGrow]">
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: load + '%' }">
          <div :class="$style.gloss"></div>
        </div>
      </div>
      <span :class="$style.percentage">{{ load }}%</span>
    </div>

    <div :class="$style.bubble">
      <div :class="$style.waveContainer">
        <div v-for="i in 3" :key="i" :class="$style.wave" :style="{ animationDelay: i * 0.2 + 's' }"></div>
      </div>
    </div>

    <div :class="[$style.bubble, $style.timeBubble]">
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

const bubbleStyles = computed(() => ({
  '--b-accent': props.color || '#f472b6', // Rosa chicle
  '--b-bg': '#ffffff'
}));
</script>

<style module>
.bubbleBar {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 20px;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(15px);
  border-radius: 100px;
  width: fit-content;
  border: 2px solid rgba(255, 255, 255, 0.5);
}

.bubble {
  background: var(--b-bg);
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 10px;
  border-radius: 50px;
  box-shadow: 0 8px 20px -6px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.8);
  transition: transform 0.3s ease;
}

.bubble:hover {
  transform: scale(1.05) translateY(-2px);
}

.flexGrow { flex-grow: 1; min-width: 150px; }

/* Indicador de pulso */
.pulseContainer { position: relative; width: 10px; height: 10px; }
.dot { width: 10px; height: 10px; background: #4ade80; border-radius: 50%; z-index: 2; position: relative; }
.pulse {
  position: absolute; inset: 0; background: #4ade80; border-radius: 50%;
  animation: ripple 2s infinite; opacity: 0.5;
}

@keyframes ripple {
  0% { transform: scale(1); opacity: 0.5; }
  100% { transform: scale(3); opacity: 0; }
}

.text { font-size: 0.8rem; font-weight: 700; color: #64748b; }

/* Barra de progreso redondeada */
.track {
  height: 10px; flex-grow: 1; background: #f1f5f9; border-radius: 20px;
  overflow: hidden; position: relative;
}
.fill {
  height: 100%; background: var(--b-accent); border-radius: 20px;
  position: relative; transition: width 1s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.gloss {
  position: absolute; top: 2px; left: 10%; width: 80%; height: 30%;
  background: rgba(255, 255, 255, 0.3); border-radius: 20px;
}

.percentage { font-size: 0.75rem; font-weight: 800; color: var(--b-accent); }

/* Ondas de conexión */
.waveContainer { display: flex; align-items: center; gap: 3px; height: 15px; }
.wave {
  width: 4px; height: 100%; background: #94a3b8; border-radius: 10px;
  animation: bounce 1s infinite alternate;
}
@keyframes bounce {
  from { height: 30%; }
  to { height: 100%; }
}

/* Tiempo con estilo de burbuja resaltada */
.timeBubble {
  background: var(--b-accent);
  border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%; /* Forma orgánica */
  color: white;
  padding: 8px 20px;
}
.timeText { font-size: 0.9rem; font-weight: 900; letter-spacing: 1px; }

</style>