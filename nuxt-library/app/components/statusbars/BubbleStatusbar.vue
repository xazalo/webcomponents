<template>
  <div v-bind="$attrs" :class="$style.bubbleRoot" :style="bubbleStyles">
    <div :class="$style.bubble">
      <div :class="$style.pulseContainer">
        <div :class="$style.dot"></div>
        <div v-if="animated" :class="$style.pulse"></div>
      </div>
      <span :class="$style.text">{{ statusLabel }}</span>
    </div>

    <div v-if="showLoad" :class="[$style.bubble, $style.flexGrow]">
      <div :class="$style.track">
        <div :class="$style.fill" :style="{ width: loadValue + '%' }">
          <div v-if="glossy" :class="$style.gloss"></div>
        </div>
      </div>
      <span :class="$style.percentage">{{ loadValue }}%</span>
    </div>

    <div :class="$style.bubble">
      <div :class="$style.waveContainer">
        <div 
          v-for="i in 3" 
          :key="i" 
          :class="$style.wave" 
          :style="{ 
            animationDelay: i * 0.2 + 's',
            background: 'var(--b-accent)'
          }"
        ></div>
      </div>
    </div>

    <div :class="[$style.bubble, $style.timeBubble]">
      <span :class="$style.timeText">{{ currentTime }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  statusLabel?: string;
  loadValue?: number;
  color?: string;       /* Color base (Pasteles recomendados) */
  // Bubble Tuning
  roundness?: string;   /* Nivel de redondez (ej: 50px o 100px) */
  glossy?: boolean;     /* ¿Efecto de brillo de plástico/caramelo? */
  animated?: boolean;   /* ¿Animaciones activas? */
  showLoad?: boolean;
  shadowIntensity?: number; /* 0 a 1 */
}

const props = withDefaults(defineProps<Props>(), {
  statusLabel: 'System Live',
  loadValue: 62,
  color: '#f472b6',
  roundness: '100px',
  glossy: true,
  animated: true,
  showLoad: true,
  shadowIntensity: 0.15
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

const bubbleStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-radius': props.roundness,
  '--b-shadow-opacity': props.shadowIntensity,
  '--b-glow': props.glossy ? 'inset 0 4px 6px rgba(255,255,255,0.6), inset 0 -4px 6px rgba(0,0,0,0.05)' : 'none',
}));
</script>

<style module>
.bubbleRoot {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 16px;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(20px);
  border-radius: 100px;
  width: fit-content;
  border: 1px solid rgba(255, 255, 255, 0.6);
}

.bubble {
  background: #ffffff;
  padding: 8px 18px;
  display: flex;
  align-items: center;
  gap: 12px;
  border-radius: var(--b-radius);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, var(--b-shadow-opacity));
  border: 1px solid rgba(255, 255, 255, 0.9);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: var(--b-glow), 0 10px 20px -5px rgba(0, 0, 0, var(--b-shadow-opacity));
}

.bubble:hover {
  transform: scale(1.05) translateY(-3px);
  background: #fff;
}

.flexGrow { flex-grow: 1; min-width: 160px; }

/* Indicador de pulso */
.pulseContainer { position: relative; width: 10px; height: 10px; }
.dot { width: 10px; height: 10px; background: #4ade80; border-radius: 50%; z-index: 2; position: relative; }
.pulse {
  position: absolute; inset: 0; background: #4ade80; border-radius: 50%;
  animation: ripple 2.5s infinite; opacity: 0.5;
}

@keyframes ripple {
  0% { transform: scale(1); opacity: 0.6; }
  100% { transform: scale(3.5); opacity: 0; }
}

.text { font-size: 0.8rem; font-weight: 800; color: #475569; letter-spacing: -0.01em; }

/* Barra de progreso redondeada */
.track {
  height: 12px; flex-grow: 1; background: #f1f5f9; border-radius: 20px;
  overflow: hidden; position: relative; box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);
}
.fill {
  height: 100%; background: var(--b-accent); border-radius: 20px;
  position: relative; transition: width 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.gloss {
  position: absolute; top: 2px; left: 5%; width: 90%; height: 35%;
  background: linear-gradient(to bottom, rgba(255,255,255,0.5), transparent);
  border-radius: 20px;
}

.percentage { font-size: 0.8rem; font-weight: 900; color: var(--b-accent); }

/* Ondas de conexión */
.waveContainer { display: flex; align-items: center; gap: 4px; height: 16px; }
.wave {
  width: 5px; height: 100%; border-radius: 10px;
  animation: bounce 0.8s infinite alternate;
  opacity: 0.8;
}
@keyframes bounce {
  from { height: 40%; transform: scaleY(0.8); }
  to { height: 100%; transform: scaleY(1.1); }
}

/* Tiempo con forma orgánica fluida */
.timeBubble {
  background: var(--b-accent);
  /* El border-radius irregular da el look "blob" */
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  color: white;
  padding: 10px 24px;
  animation: morph 6s ease-in-out infinite;
}

@keyframes morph {
  0%, 100% { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
  50% { border-radius: 30% 60% 70% 40% / 50% 60% 30% 60%; }
}

.timeText { font-size: 0.95rem; font-weight: 900; filter: drop-shadow(0 2px 2px rgba(0,0,0,0.1)); }

</style>