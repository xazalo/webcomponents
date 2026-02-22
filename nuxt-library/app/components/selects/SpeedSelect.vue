<template>
  <div v-bind="$attrs" :class="$style.speedRoot" :style="speedStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen]" 
        @click="isOpen = !isOpen"
      >
        <div :class="$style.scanline"></div>
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <div :class="$style.arrowContainer">
          <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
            <path d="M7 13l5 5 5-5M7 6l5 5 5-5"/>
          </svg>
        </div>
      </button>
      
      <Transition name="speed-drop">
        <div v-if="isOpen" :class="$style.menu">
          <div 
            v-for="opt in options" 
            :key="opt.value" 
            :class="[$style.item, modelValue === opt.value && $style.itemSelected]"
            @click="selectOption(opt.value)"
          >
            <span :class="$style.itemText">{{ opt.label }}</span>
            <div :class="$style.itemGlow"></div>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
const props = defineProps<{ 
  modelValue: any; 
  options: { label: string, value: any }[]; 
  placeholder?: string; 
  color?: string; 
}>();

const emit = defineEmits(['update:modelValue']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);
const selectOption = (v: any) => { emit('update:modelValue', v); isOpen.value = false; };

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#00ff41', // Verde Matrix / Racing
  '--s-skew': '-15deg',
}));
</script>

<style module>
.speedRoot { width: 100%; font-family: 'Orbitron', sans-serif, system-ui; }
.container { position: relative; }

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.6rem 1.2rem;
  background: #000;
  border: 1px solid #333;
  border-left: 4px solid var(--s-accent);
  color: #fff;
  font-weight: 900;
  text-transform: uppercase;
  font-style: italic;
  letter-spacing: 1px;
  cursor: pointer;
  transform: skewX(var(--s-skew));
  position: relative;
  overflow: hidden;
  transition: all 0.2s ease;
}

.trigger:hover {
  background: #111;
  border-color: var(--s-accent);
  box-shadow: -5px 0 15px color-mix(in srgb, var(--s-accent) 30%, transparent);
}

.triggerOpen {
  background: var(--s-accent);
  color: #000;
  border-color: #fff;
}

.label { 
  transform: skewX(calc(var(--s-skew) * -1)); /* Enderezar texto */
  font-size: 0.8rem;
}

.scanline {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(255,255,255,0.05) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.icon { 
  transition: transform 0.3s cubic-bezier(0.19, 1, 0.22, 1);
  transform: skewX(calc(var(--s-skew) * -1)); 
}
.iconRotated { transform: skewX(calc(var(--s-skew) * -1)) rotate(180deg); }

.menu {
  position: absolute;
  top: calc(100% + 10px);
  width: 100%;
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(10px);
  border: 1px solid #333;
  z-index: 100;
  transform: skewX(var(--s-skew));
  padding: 4px;
}

.item {
  padding: 0.8rem 1rem;
  cursor: pointer;
  position: relative;
  transition: all 0.2s ease;
  overflow: hidden;
  border-bottom: 1px solid rgba(255,255,255,0.05);
}

.itemText {
  display: block;
  transform: skewX(calc(var(--s-skew) * -1));
  font-size: 0.75rem;
  font-weight: 700;
  color: #aaa;
  z-index: 2;
  position: relative;
}

.item:hover .itemText {
  color: #fff;
  transform: skewX(calc(var(--s-skew) * -1)) translateX(5px);
}

.itemGlow {
  position: absolute; inset: 0;
  background: var(--s-accent);
  opacity: 0;
  transition: opacity 0.2s;
  z-index: 1;
}

.item:hover .itemGlow { opacity: 0.15; }

.itemSelected .itemText {
  color: var(--s-accent);
  text-shadow: 0 0 8px var(--s-accent);
}

/* Animación Nitro */
:global(.speed-drop-enter-active) {
  animation: nitroDrop 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

@keyframes nitroDrop {
  from { opacity: 0; transform: skewX(var(--s-skew)) translateX(-20px); }
  to { opacity: 1; transform: skewX(var(--s-skew)) translateX(0); }
}
</style>