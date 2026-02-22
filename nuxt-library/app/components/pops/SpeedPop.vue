<template>
  <Teleport to="body">
    <Transition name="speed-nitro">
      <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
        <div v-bind="$attrs" :class="$style.speedModal" :style="speedStyles">
          <div :class="$style.wing"></div>
          
          <div :class="$style.inner">
            <div :class="$style.header">
              <Icon name="lucide:gauge" :class="$style.icon" />
              <span :class="$style.status">Sys</span>
            </div>

            <div :class="$style.content">
              <slot />
            </div>
            
            <button @click="$emit('update:modelValue', false)" :class="$style.nitroAction">
              <span>{{ buttonText }}</span>
              <div :class="$style.glowLine"></div>
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps<{ modelValue: boolean; color?: string, buttonText: string }>();

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#ff004c', // Rojo racing
  '--s-skew': '-12deg',
}));
</script>

<style module>
.overlay {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(4px);
  display: flex; align-items: center; justify-content: center;
  z-index: 9999;
}

.speedModal {
  background: #0a0a0a;
  color: white;
  width: 95%;
  max-width: 420px;
  padding: 2.5rem;
  position: relative;
  border-left: 5px solid var(--s-accent);
  /* Inclinación aerodinámica */
  transform: skewX(var(--s-skew));
  box-shadow: 20px 0 50px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

.wing {
  position: absolute;
  top: 0; right: 0;
  width: 40px; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.05));
  border-right: 1px solid rgba(255, 255, 255, 0.1);
}

.inner {
  /* Re-enderezamos el contenido para legibilidad */
  transform: skewX(calc(var(--s-skew) * -1));
  display: flex; flex-direction: column; gap: 1.5rem;
}

.header {
  display: flex; align-items: center; gap: 0.5rem;
  font-weight: 900; font-style: italic; color: var(--s-accent);
  text-transform: uppercase; font-size: 0.8rem;
}

.icon { width: 1.2rem; height: 1.2rem; }

.content {
  font-size: 1.2rem; font-weight: 700; font-style: italic;
  line-height: 1.3; color: #eee;
}

.nitroAction {
  background: #111;
  color: white;
  border: 1px solid #333;
  padding: 1rem;
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: all 0.2s ease;
}

.nitroAction:hover {
  background: var(--s-accent);
  border-color: #fff;
  transform: scale(1.02) translateX(5px);
}

.glowLine {
  position: absolute; bottom: 0; left: -100%;
  width: 100%; height: 2px;
  background: #fff;
  transition: left 0.3s ease;
}

.nitroAction:hover .glowLine { left: 100%; }

/* Animación Nitro Entry */
:global(.speed-nitro-enter-active) {
  animation: nitroIn 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

@keyframes nitroIn {
  0% { transform: translateX(-100px) skewX(-30deg); opacity: 0; }
  100% { transform: translateX(0) skewX(var(--s-skew)); opacity: 1; }
}
</style>