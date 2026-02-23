<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.speedMenu, $style[size]]" 
    :style="speedStyles"
  >
    <div :class="[$style.cell, $style.headerCell]">
      <div v-if="effects" :class="$style.stripe" />
      <div :class="$style.staticContent">
        <span :class="$style.label">{{ prefix }}</span>
        <h2 :class="$style.title">{{ activeLabel }}</h2>
      </div>
    </div>

    <div :class="$style.list">
      <a 
        v-for="(link, index) in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          { [$style.active]: modelValue === link.path }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div v-if="nitro" :class="$style.speedLine" />
        
        <div :class="$style.staticContent">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="$style.rank">
          {{ (index + 1).toString().padStart(2, '0') }}
        </div>
      </a>
    </div>

    <button 
      v-if="buttonText"
      :class="[$style.cell, $style.actionBtn]" 
      @click="$emit('action')"
    >
      <span :class="$style.staticContent">{{ buttonText }}</span>
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
  prefix?: string;
  // Configuración de Rendimiento
  primaryColor?: string;  /* Color del equipo (ej: Rojo Racing) */
  nitroColor?: string;    /* Color del rastro (ej: Verde Neón) */
  angle?: number;         /* Inclinación en grados (sugerido: -10 a -15) */
  size?: 'sm' | 'md' | 'lg';
  nitro?: boolean;        /* ¿Línea de velocidad al hacer hover? */
  effects?: boolean;      /* ¿Rayas decorativas en el header? */
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '',
  prefix: 'DASHBOARD_V8',
  primaryColor: '#ff3e3e',
  nitroColor: '#00ff41',
  angle: -10,
  size: 'md',
  nitro: true,
  effects: true
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'OVERVIEW';
});

const speedStyles = computed(() => ({
  '--s-primary': props.primaryColor,
  '--s-nitro': props.nitroColor,
  '--s-angle': `${props.angle}deg`,
  '--s-angle-inv': `${props.angle * -1}deg`,
  '--s-bg': '#121212'
}));
</script>

<style module>
.speedMenu {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 20px;
  background: var(--s-bg);
  width: 100%;
  max-width: 400px;
  /* Textura de pista / fibra de carbono */
  background-image: 
    radial-gradient(circle at 2px 2px, rgba(255,255,255,0.03) 1px, transparent 0);
  background-size: 8px 8px;
  font-family: 'Arial Black', sans-serif;
}

.cell {
  background: #1a1a1a;
  padding: 15px 25px;
  border: none;
  position: relative;
  transform: skewX(var(--s-angle));
  transition: all 0.25s cubic-bezier(0.23, 1, 0.32, 1);
  overflow: hidden;
}

/* El contenido se endereza para no perder legibilidad */
.staticContent {
  transform: skewX(var(--s-angle-inv));
}

/* --- Header Section --- */
.headerCell {
  background: linear-gradient(90deg, var(--s-primary) 0%, #000 150%);
  border-left: 6px solid #fff;
  margin-bottom: 12px;
}

.stripe {
  position: absolute;
  top: 0; right: 20px; width: 40px; height: 100%;
  background: rgba(255,255,255,0.1);
  transform: skewX(25deg);
}

.label {
  font-size: 0.65rem;
  font-style: italic;
  color: rgba(255,255,255,0.8);
  letter-spacing: 1px;
}

.title {
  margin: 0;
  font-size: 1.8rem;
  font-weight: 900;
  font-style: italic;
  color: #fff;
  text-transform: uppercase;
  line-height: 1;
}

/* --- List & Links --- */
.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
  border-right: 4px solid transparent;
}

.linkCell:hover {
  background: #2a2a2a;
  transform: skewX(var(--s-angle)) translateX(10px);
  border-right-color: var(--s-primary);
}

.active {
  background: #fff !important;
  transform: skewX(var(--s-angle)) scale(1.04);
  box-shadow: -15px 0 30px rgba(0,0,0,0.5);
}

.active .linkTitle, .active .linkDesc, .active .rank {
  color: #000 !important;
}

.linkTitle {
  display: block;
  font-weight: 900;
  font-size: 1.25rem;
  font-style: italic;
  color: #fff;
  text-transform: uppercase;
  line-height: 1;
}

.linkDesc {
  font-size: 0.7rem;
  color: #666;
  font-weight: 800;
}

.rank {
  font-family: 'Courier New', monospace;
  font-weight: 900;
  color: var(--s-primary);
  transform: skewX(var(--s-angle-inv)) scale(1.2);
}

/* --- FX: Speed Line (Nitro) --- */
.speedLine {
  position: absolute;
  bottom: 0; left: 0; 
  width: 0; height: 3px;
  background: var(--s-nitro);
  box-shadow: 0 0 10px var(--s-nitro);
  transition: width 0.4s ease;
}

.linkCell:hover .speedLine {
  width: 100%;
}

/* --- Action Button --- */
.actionBtn {
  margin-top: 15px;
  background: transparent;
  border: 2px solid var(--s-primary);
  color: var(--s-primary);
  font-weight: 900;
  font-style: italic;
  cursor: pointer;
  text-transform: uppercase;
}

.actionBtn:hover {
  background: var(--s-primary);
  color: #fff;
}

/* --- Tamaños --- */
.sm .cell { padding: 10px 18px; }
.sm .title { font-size: 1.4rem; }
.lg .cell { padding: 20px 32px; }
.lg .title { font-size: 2.2rem; }
</style>