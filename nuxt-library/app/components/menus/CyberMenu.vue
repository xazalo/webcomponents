<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.cyberMenu, $style[size]]" 
    :style="cyberStyles"
  >
    <div :class="[$style.cell, $style.headerCell]">
      <div v-if="scanning" :class="$style.scanline" />
      <span :class="$style.label">{{ statusText }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
      <div :class="$style.cornerDeco" />
    </div>

    <div :class="$style.list">
      <a 
        v-for="(link, index) in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          { [$style.active]: modelValue === link.path, [$style.glitchOnHover]: glitch }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <div :class="$style.topRow">
            <span :class="$style.index">ID_{{ (index + 1).toString().padStart(2, '0') }}</span>
            <span :class="$style.linkTitle">{{ link.label }}</span>
          </div>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="$style.indicator">
          <div :class="$style.pixelSquare" />
        </div>
      </a>
    </div>

    <button 
      v-if="buttonText"
      :class="[$style.cell, $style.logoutBtn]"
      @click="$emit('action')"
    >
      <span :class="$style.btnText">{{ buttonText }}</span>
      <span :class="$style.warning">!</span>
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

interface Props {
  modelValue: string;
  links: NavLink[];
  buttonText?: string;
  statusText?: string;
  // Personalización Cyber
  accentColor?: string;   /* Color neón (Cyan, Amarillo, Magenta) */
  baseColor?: string;     /* Fondo de las celdas */
  size?: 'sm' | 'md' | 'lg';
  glitch?: boolean;       /* Habilita micro-vibraciones al pasar el mouse */
  scanning?: boolean;     /* Líneas de escaneo animadas en el header */
  cutSize?: number;       /* Tamaño del chaflán en px */
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'TERMINATE_SESSION',
  statusText: 'CORE_LINK_ESTABLISHED',
  accentColor: '#00f0ff',
  baseColor: '#1a1a1b',
  size: 'md',
  glitch: true,
  scanning: true,
  cutSize: 10
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'OVERVIEW';
});

const cyberStyles = computed(() => ({
  '--cy-accent': props.accentColor,
  '--cy-bg-cell': props.baseColor,
  '--cy-cut': `${props.cutSize}px`,
  '--cy-glow': `${props.accentColor}44`,
}));
</script>

<style module>
.cyberMenu {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 12px;
  background: #0d0d0f;
  width: 100%;
  max-width: 360px;
  border-right: 3px solid var(--cy-accent);
  font-family: 'JetBrains Mono', 'Consolas', monospace;
}

.cell {
  background: var(--cy-bg-cell);
  padding: 14px 18px;
  border: 1px solid #333;
  position: relative;
  overflow: hidden;
  /* Corte dinámico basado en props */
  clip-path: polygon(
    0 0, 
    100% 0, 
    100% calc(100% - var(--cy-cut)), 
    calc(100% - var(--cy-cut)) 100%, 
    0 100%
  );
  transition: all 0.2s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

/* --- Header HUD --- */
.headerCell {
  background: #000;
  border-left: 4px solid var(--cy-accent);
}

.scanline {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 50%, var(--cy-glow) 50%);
  background-size: 100% 4px;
  animation: scroll 10s linear infinite;
  pointer-events: none;
}

@keyframes scroll {
  from { background-position: 0 0; }
  to { background-position: 0 100%; }
}

.label {
  font-size: 0.6rem;
  font-weight: bold;
  color: var(--cy-accent);
  letter-spacing: 2px;
  text-transform: uppercase;
}

.title {
  margin: 4px 0 0;
  font-size: 1.5rem;
  font-weight: 900;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: -1px;
}

/* --- List Items --- */
.list {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.linkCell {
  text-decoration: none;
  cursor: pointer;
  border-color: #333;
}

.linkCell:hover {
  background: #252528;
  border-color: var(--cy-accent);
  transform: translateX(6px);
  box-shadow: -4px 0 15px var(--cy-glow);
}

.glitchOnHover:hover .content {
  animation: glitch-anim 0.2s infinite;
}

@keyframes glitch-anim {
  0% { transform: translate(0); }
  20% { transform: translate(-2px, 1px); }
  40% { transform: translate(-2px, -1px); }
  60% { transform: translate(2px, 1px); }
  80% { transform: translate(2px, -1px); }
  100% { transform: translate(0); }
}

.active {
  background: var(--cy-accent) !important;
  clip-path: polygon(var(--cy-cut) 0, 100% 0, 100% 100%, 0 100%, 0 var(--cy-cut));
}

.active .linkTitle, .active .linkDesc, .active .index {
  color: #000 !important;
}

.topRow {
  display: flex;
  align-items: baseline;
  gap: 10px;
}

.index {
  font-size: 0.7rem;
  color: var(--cy-accent);
  font-weight: bold;
}

.linkTitle {
  font-weight: 900;
  font-size: 1.1rem;
  color: #fff;
  text-transform: uppercase;
}

.linkDesc {
  display: block;
  font-size: 0.65rem;
  color: #888;
  margin-top: 2px;
}

.pixelSquare {
  width: 8px;
  height: 8px;
  background: var(--cy-accent);
  box-shadow: 0 0 10px var(--cy-accent);
}

.active .pixelSquare {
  background: #000;
  box-shadow: none;
}

/* --- Logout / Emergency --- */
.logoutBtn {
  margin-top: 10px;
  background: #ff003c; /* Magenta Cyber */
  border: none;
  color: #fff;
  font-weight: 900;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logoutBtn:hover {
  filter: brightness(1.2);
  box-shadow: 0 0 20px rgba(255, 0, 60, 0.4);
}

.warning {
  background: #000;
  color: #ff003c;
  width: 18px;
  height: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  font-weight: bold;
}

/* --- Corner Detail --- */
.cornerDeco {
  position: absolute;
  top: 0; right: 0;
  width: 25px; height: 3px;
  background: var(--cy-accent);
}

/* --- Tamaños --- */
.sm { max-width: 300px; }
.sm .title { font-size: 1.2rem; }
.lg { max-width: 420px; }
.lg .title { font-size: 1.8rem; }
</style>