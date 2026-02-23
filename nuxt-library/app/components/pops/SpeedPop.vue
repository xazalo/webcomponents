<template>
  <Teleport to="body">
    <Transition :name="$style.speedNitro">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.speedModal, $style[size]]" 
          :style="speedStyles"
        >
          <div v-if="texture" :class="$style.textureOverlay" />
          
          <div :class="$style.wing">
            <div v-for="i in 3" :key="i" :class="$style.wingStripe" />
          </div>
          
          <div :class="$style.inner">
            <header :class="$style.header">
              <div :class="$style.statusBadge">
                <span :class="$style.statusDot" />
                <span>{{ label }}</span>
              </div>
              <div :class="$style.lineDecor" />
            </header>

            <div :class="$style.content">
              <slot />
            </div>
            
            <footer :class="$style.footer">
              <slot name="actions">
                <button @click="handleClose" :class="$style.nitroAction">
                  <span :class="$style.btnText">{{ buttonText }}</span>
                  <div :class="$style.glowLine"></div>
                  <div :class="$style.nitroFlare"></div>
                </button>
              </slot>
            </footer>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  buttonText?: string;
  label?: string;
  color?: string;
  // Configuración Racing
  skew?: number;          /* Ángulo de inclinación (ej: -12) */
  texture?: boolean;      /* Rejilla de competición */
  nitroBlur?: boolean;    /* Desenfoque de movimiento en el overlay */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'SHIFT UP',
  label: 'RACE_LOG // 01',
  color: '#ff004c',
  skew: -12,
  texture: true,
  nitroBlur: true,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const speedStyles = computed(() => ({
  '--s-accent': props.color,
  '--s-skew': `${props.skew}deg`,
  '--s-unskew': `${props.skew * -1}deg`,
  '--s-overlay-blur': props.nitroBlur ? '6px' : '0px',
}));
</script>

<style module>
/* --- Animación Nitro Entry --- */
.speedNitroEnterActive {
  animation: nitroIn 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
}
.speedNitroLeaveActive {
  transition: all 0.2s ease-in;
  transform: translateX(100px) skewX(var(--s-skew));
  opacity: 0;
}

@keyframes nitroIn {
  0% { transform: translateX(-150px) skewX(-40deg); opacity: 0; filter: blur(10px); }
  70% { transform: translateX(10px) skewX(var(--s-skew)); }
  100% { transform: translateX(0) skewX(var(--s-skew)); opacity: 1; filter: blur(0); }
}

/* --- Overlay --- */
.overlay {
  position: fixed; inset: 0;
  background: rgba(5, 5, 5, 0.9);
  backdrop-filter: blur(var(--s-overlay-blur));
  display: flex; align-items: center; justify-content: center;
  z-index: 9999; padding: 20px;
}

/* --- Chasis del Modal --- */
.speedModal {
  background: #0f0f11;
  color: white;
  width: 100%;
  padding: 2.5rem;
  position: relative;
  border-left: 6px solid var(--s-accent);
  transform: skewX(var(--s-skew));
  box-shadow: 25px 0 60px rgba(0, 0, 0, 0.8);
  overflow: hidden;
  border-bottom: 1px solid #222;
}

.sm { max-width: 360px; }
.md { max-width: 440px; }
.lg { max-width: 620px; }

/* --- Textura y Wing --- */
.textureOverlay {
  position: absolute; inset: 0;
  background-image: radial-gradient(#222 1px, transparent 0);
  background-size: 4px 4px;
  opacity: 0.5;
  pointer-events: none;
}

.wing {
  position: absolute; top: 0; right: 0;
  width: 50px; height: 100%;
  display: flex; flex-direction: column; justify-content: space-around;
  padding: 20px 0; opacity: 0.2;
}

.wingStripe {
  width: 100%; height: 2px; background: white;
}

/* --- Contenido --- */
.inner {
  transform: skewX(var(--s-unskew));
  display: flex; flex-direction: column; gap: 1.8rem;
}

.header {
  display: flex; align-items: center; gap: 1rem;
}

.statusBadge {
  display: flex; align-items: center; gap: 8px;
  font-family: 'Arial Black', sans-serif;
  font-weight: 900; font-style: italic; color: var(--s-accent);
  text-transform: uppercase; font-size: 0.75rem;
  letter-spacing: 1px;
}

.statusDot {
  width: 6px; height: 6px; background: var(--s-accent);
  border-radius: 50%; box-shadow: 0 0 8px var(--s-accent);
}

.lineDecor { flex: 1; height: 1px; background: #333; }

.content {
  font-size: 1.25rem; font-weight: 800; font-style: italic;
  line-height: 1.4; color: #fff;
  font-family: 'Inter', sans-serif;
}

/* --- Botón Nitro --- */
.nitroAction {
  background: #1a1b1e;
  color: white;
  border: 1px solid #333;
  padding: 1.2rem;
  width: 100%;
  font-weight: 900; font-style: italic;
  text-transform: uppercase;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
}

.btnText { position: relative; z-index: 2; }

.nitroAction:hover {
  background: var(--s-accent);
  border-color: #fff;
  transform: translateX(8px);
}

.glowLine {
  position: absolute; bottom: 0; left: -100%;
  width: 100%; height: 3px; background: #fff;
  transition: left 0.4s ease;
}

.nitroAction:hover .glowLine { left: 100%; }

.nitroFlare {
  position: absolute; inset: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transform: translateX(-100%);
  transition: transform 0.5s;
}

.nitroAction:hover .nitroFlare { transform: translateX(100%); }
</style>