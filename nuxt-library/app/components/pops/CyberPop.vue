<template>
  <Teleport to="body">
    <Transition :name="$style.cyberGlitch">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.cyberModal, $style[size]]" 
          :style="cyberStyles"
        >
          <div v-if="scanlines" :class="$style.scanline" />
          
          <header :class="$style.header">
            <div :class="$style.headerBg" />
            <div :class="$style.titleWrapper">
              <span :class="$style.tag">SYS_MSG</span>
              <span :class="$style.title">{{ title }}</span>
            </div>
            <button @click="handleClose" :class="$style.closeBtn">
              <span :class="$style.closeText">ESC</span>
              <span :class="$style.closeCross">×</span>
            </button>
          </header>

          <div :class="$style.content">
            <slot />
          </div>

          <footer v-if="$slots.actions" :class="$style.footer">
            <div :class="$style.footerDecor" />
            <slot name="actions" />
          </footer>

          <div :class="[$style.corner, $style.topLeft]" />
          <div :class="[$style.corner, $style.bottomRight]" />
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
  title?: string;
  color?: string;        /* Color del Neón (ej: #00ff41, #fdee06) */
  size?: 'sm' | 'md' | 'lg';
  scanlines?: boolean;   /* ¿Líneas de interferencia de TV antigua? */
  glitch?: boolean;      /* ¿Animación de entrada con glitch? */
  cutSize?: string;      /* Tamaño del recorte de las esquinas (ej: 20px) */
}

const props = withDefaults(defineProps<Props>(), {
  title: 'ENCRYPTED_DATA',
  color: '#00f3ff',
  size: 'md',
  scanlines: true,
  glitch: true,
  cutSize: '20px'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const cyberStyles = computed(() => ({
  '--c-neon': props.color,
  '--c-cut': props.cutSize,
  '--c-glitch': props.glitch ? 'cyberGlitch 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94) both' : 'none'
}));
</script>

<style module>
/* --- Animaciones Cyber --- */
.cyberGlitchEnterActive { animation: var(--c-glitch); }
.cyberGlitchLeaveActive { opacity: 0; transform: scale(0.9); transition: all 0.2s; }

@keyframes cyberGlitch {
  0% { transform: translate(0); opacity: 0; }
  20% { transform: translate(-5px, 5px); opacity: 0.5; }
  40% { transform: translate(-5px, -5px); opacity: 0.7; }
  60% { transform: translate(5px, 5px); opacity: 0.8; }
  80% { transform: translate(5px, -5px); opacity: 0.9; }
  100% { transform: translate(0); opacity: 1; }
}

/* --- Overlay --- */
.overlay {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(8px);
  display: flex; align-items: center; justify-content: center;
  z-index: 9999; padding: 20px;
}

/* --- Estructura Modal --- */
.cyberModal {
  background: #0d0d0d;
  color: #fff;
  width: 100%;
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.1);
  /* El famoso recorte asimétrico Cyber */
  clip-path: polygon(
    var(--c-cut) 0%, 100% 0%, 100% calc(100% - var(--c-cut)), 
    calc(100% - var(--c-cut)) 100%, 0% 100%, 0% var(--c-cut)
  );
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5), inset 0 0 10px rgba(var(--c-neon), 0.1);
}

.sm { max-width: 350px; }
.md { max-width: 500px; }
.lg { max-width: 700px; }

/* --- Scanlines --- */
.scanline {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(0, 0, 0, 0.3) 50%);
  background-size: 100% 4px;
  pointer-events: none; z-index: 10;
  opacity: 0.3;
}

/* --- Header --- */
.header {
  padding: 1.2rem; position: relative;
  display: flex; justify-content: space-between; align-items: center;
  border-bottom: 2px solid var(--c-neon);
}

.headerBg {
  position: absolute; inset: 0;
  background: var(--c-neon); opacity: 0.05;
}

.titleWrapper { display: flex; flex-direction: column; z-index: 2; }

.tag {
  font-size: 0.6rem; color: var(--c-neon);
  font-family: monospace; letter-spacing: 2px;
}

.title {
  font-weight: 900; text-transform: uppercase;
  letter-spacing: 2px; font-size: 1.1rem;
  text-shadow: 0 0 10px var(--c-neon);
}

.closeBtn {
  background: none; border: none; color: #666;
  cursor: pointer; display: flex; align-items: center; gap: 10px;
  z-index: 2; transition: color 0.2s;
}

.closeBtn:hover { color: var(--c-neon); }

.closeText { font-size: 0.7rem; font-family: monospace; }
.closeCross { font-size: 1.8rem; line-height: 1; }

/* --- Cuerpo --- */
.content {
  padding: 2rem; line-height: 1.6; position: relative;
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  font-size: 0.95rem; color: #bbb;
}

/* --- Footer --- */
.footer {
  padding: 1rem 2rem; background: rgba(255, 255, 255, 0.02);
  display: flex; justify-content: flex-end; gap: 1rem;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

/* --- Adornos HUD --- */
.corner {
  position: absolute; width: 15px; height: 15px;
  border: 2px solid var(--c-neon); z-index: 5;
}

.topLeft { top: 0; left: 0; border-right: none; border-bottom: none; }
.bottomRight { bottom: 0; right: 0; border-left: none; border-top: none; }
</style>