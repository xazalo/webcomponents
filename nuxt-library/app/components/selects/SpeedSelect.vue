<template>
  <div v-bind="$attrs" :class="$style.speedRoot" :style="speedStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
        :disabled="disabled"
      >
        <div v-if="animated" :class="$style.speedLines"></div>
        <div :class="$style.scanline"></div>
        
        <div :class="$style.content">
          <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        </div>

        <div :class="$style.arrowContainer">
          <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
            <path d="M7 13l5 5 5-5M7 6l5 5 5-5"/>
          </svg>
        </div>
      </button>
      
      <Transition :name="$style.nitroAnim">
        <div v-if="isOpen" :class="[$style.menu, $style[`align-${menuAlign}`]]">
          <div :class="$style.menuHeader" v-if="menuTitle">{{ menuTitle }}</div>
          <div :class="$style.optionsList">
            <div 
              v-for="opt in options" 
              :key="opt.value" 
              :class="[
                $style.item, 
                modelValue === opt.value && $style.itemSelected,
                opt.disabled && $style.itemDisabled
              ]"
              @click="!opt.disabled && selectOption(opt.value)"
            >
              <div :class="$style.itemGlow"></div>
              <span :class="$style.itemText">{{ opt.label }}</span>
              <span v-if="modelValue === opt.value" :class="$style.activeTag">READY</span>
            </div>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: any;
  options: { label: string; value: any; disabled?: boolean }[];
  placeholder?: string;
  color?: string;       /* Color Nitro (ej: #00ff41) */
  // Tuning Props
  skew?: number;        /* Ángulo de inclinación (de -5 a -25) */
  nitro?: boolean;      /* Efecto de brillo intenso */
  animated?: boolean;   /* ¿Líneas de velocidad animadas? */
  size?: 'sm' | 'md' | 'lg';
  menuAlign?: 'left' | 'right';
  menuTitle?: string;
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'SELECT_DRIVE',
  color: '#00ff41',
  skew: -15,
  nitro: true,
  animated: true,
  size: 'md',
  menuAlign: 'left'
});

const emit = defineEmits(['update:modelValue', 'change']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);

const selectOption = (v: any) => {
  emit('update:modelValue', v);
  emit('change', v);
  isOpen.value = false;
};

const speedStyles = computed(() => ({
  '--s-accent': props.color,
  '--s-skew': `${props.skew}deg`,
  '--s-skew-inv': `${props.skew * -1}deg`,
  '--s-glow': props.nitro ? `0 0 15px color-mix(in srgb, ${props.color} 50%, transparent)` : 'none',
}));
</script>

<style module>
/* --- Animación Nitro Drop --- */
.nitroAnimEnterActive {
  animation: nitroIn 0.25s cubic-bezier(0.23, 1, 0.32, 1);
}
.nitroAnimLeaveActive {
  transition: all 0.2s ease-in;
  opacity: 0;
  transform: skewX(var(--s-skew)) translateX(20px);
}

@keyframes nitroIn {
  from { opacity: 0; transform: skewX(var(--s-skew)) translateX(-40px); filter: blur(10px); }
  to { opacity: 1; transform: skewX(var(--s-skew)) translateX(0); filter: blur(0); }
}

.speedRoot { width: 100%; font-family: 'Orbitron', sans-serif; }
.container { position: relative; }

/* --- Trigger --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #000;
  border: 1px solid #333;
  border-left: 4px solid var(--s-accent);
  color: #fff;
  font-weight: 900;
  text-transform: uppercase;
  font-style: italic;
  cursor: pointer;
  transform: skewX(var(--s-skew));
  position: relative;
  overflow: hidden;
  transition: all 0.2s ease;
  box-shadow: var(--s-glow);
}

.sm { padding: 0.5rem 1rem; }
.md { padding: 0.7rem 1.4rem; }
.lg { padding: 1rem 2rem; }

.trigger:hover:not(:disabled) {
  background: #111;
  border-color: var(--s-accent);
  transform: skewX(var(--s-skew)) scale(1.02);
}

.triggerOpen {
  background: var(--s-accent);
  color: #000;
  border-color: #fff;
}

.content { transform: skewX(var(--s-skew-inv)); }
.label { font-size: 0.85rem; letter-spacing: 1px; }

/* Líneas de velocidad */
.speedLines {
  position: absolute; inset: 0;
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.05) 50%, transparent 100%);
  background-size: 200% 100%;
  animation: moveLines 1s linear infinite;
}

@keyframes moveLines {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.scanline {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(255,255,255,0.03) 50%);
  background-size: 100% 4px;
}

.icon { 
  transition: transform 0.3s cubic-bezier(0.19, 1, 0.22, 1);
  transform: skewX(var(--s-skew-inv)); 
  color: var(--s-accent);
}
.triggerOpen .icon { color: #000; transform: skewX(var(--s-skew-inv)) rotate(180deg); }

/* --- Dropdown --- */
.menu {
  position: absolute;
  top: calc(100% + 12px);
  width: 100%;
  background: rgba(5, 5, 5, 0.95);
  backdrop-filter: blur(12px);
  border: 1px solid #333;
  border-right: 4px solid var(--s-accent);
  z-index: 100;
  transform: skewX(var(--s-skew));
  padding: 5px;
}

.menuHeader {
  transform: skewX(var(--s-skew-inv));
  font-size: 0.6rem; color: var(--s-accent);
  padding: 5px 10px; font-weight: 800; letter-spacing: 2px;
}

.item {
  padding: 0.8rem 1.2rem;
  cursor: pointer;
  position: relative;
  transition: all 0.2s ease;
  overflow: hidden;
  display: flex; justify-content: space-between; align-items: center;
}

.itemText {
  transform: skewX(var(--s-skew-inv));
  font-size: 0.75rem; font-weight: 700; color: #888;
  z-index: 2; transition: all 0.2s;
}

.item:hover:not(.itemDisabled) .itemText {
  color: #fff;
  transform: skewX(var(--s-skew-inv)) translateX(8px);
}

.itemGlow {
  position: absolute; inset: 0;
  background: linear-gradient(90deg, var(--s-accent), transparent);
  opacity: 0; transition: opacity 0.2s; z-index: 1;
}

.item:hover:not(.itemDisabled) .itemGlow { opacity: 0.2; }

.itemSelected { background: rgba(255, 255, 255, 0.05); }
.itemSelected .itemText { color: var(--s-accent); text-shadow: 0 0 10px var(--s-accent); }

.activeTag {
  transform: skewX(var(--s-skew-inv));
  font-size: 0.6rem; background: var(--s-accent); color: #000;
  padding: 2px 6px; font-weight: 900;
}

.itemDisabled { opacity: 0.2; cursor: not-allowed; }
</style>