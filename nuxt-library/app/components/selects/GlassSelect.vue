<template>
  <div v-bind="$attrs" :class="$style.glassRoot" :style="glassStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
      >
        <div :class="$style.rimLight" />
        <span :class="$style.val">{{ selectedLabel || placeholder }}</span>
        
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
          <path d="m7 15 5 5 5-5M7 9l5-5 5 5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
      
      <Transition :name="$style.glassSlide">
        <div v-if="isOpen" :class="$style.dropdown">
          <div :class="$style.scrollArea">
            <div 
              v-for="opt in options" 
              :key="opt.value" 
              :class="[
                $style.opt, 
                modelValue === opt.value && $style.optActive,
                opt.disabled && $style.optDisabled
              ]"
              @click.stop="!opt.disabled && selectOption(opt.value)"
            >
              <div :class="$style.optContent">
                <span :class="$style.optText">{{ opt.label }}</span>
                <span v-if="modelValue === opt.value" :class="$style.checkBadge">SELECTED</span>
              </div>
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
  // Configuración Glass
  blur?: string;
  opacity?: number;
  saturation?: string;
  accentColor?: string; /* Color para resaltar la selección */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Elige una opción...',
  blur: '18px',
  opacity: 0.5,
  saturation: '160%',
  accentColor: '#3b82f6',
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'change']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);

const selectOption = (v: any) => {
  emit('update:modelValue', v);
  emit('change', v);
  isOpen.value = false;
};

const glassStyles = computed(() => ({
  '--g-blur': props.blur,
  '--g-opacity': props.opacity,
  '--g-saturation': props.saturation,
  '--g-accent': props.accentColor,
}));
</script>

<style module>
/* --- Animación Premium --- */
.glassSlideEnterActive, .glassSlideLeaveActive {
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}
.glassSlideEnterFrom, .glassSlideLeaveTo {
  opacity: 0;
  transform: translateY(10px) scale(0.95);
}

.glassRoot { width: 100%; font-family: 'Inter', system-ui, sans-serif; }
.container { position: relative; }

/* --- Trigger Principal --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(255, 255, 255, var(--g-opacity));
  backdrop-filter: blur(var(--g-blur)) saturate(var(--g-saturation));
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(var(--g-saturation));
  border: 1px solid rgba(255, 255, 255, 0.4);
  border-radius: 12px;
  cursor: pointer;
  position: relative;
  transition: all 0.2s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.sm { padding: 0.5rem 0.8rem; font-size: 0.85rem; }
.md { padding: 0.7rem 1.2rem; font-size: 0.95rem; }
.lg { padding: 1rem 1.5rem; font-size: 1.1rem; }

.rimLight {
  position: absolute; inset: 0;
  border-radius: inherit;
  background: linear-gradient(135deg, rgba(255,255,255,0.4) 0%, transparent 50%);
  pointer-events: none;
}

.trigger:hover {
  background: rgba(255, 255, 255, calc(var(--g-opacity) + 0.1));
  border-color: rgba(255, 255, 255, 0.6);
}

.triggerOpen {
  border-color: var(--g-accent);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--g-accent) 20%, transparent);
}

.val { font-weight: 700; color: #111827; z-index: 1; }
.icon { opacity: 0.5; transition: transform 0.3s ease; color: #111827; }
.iconRotated { transform: rotate(180deg); opacity: 1; color: var(--g-accent); }

/* --- Listado de Opciones (Menu) --- */
.dropdown {
  position: absolute;
  top: calc(100% + 8px);
  left: 0; width: 100%;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(30px) saturate(200%);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 14px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.12);
  z-index: 999;
  overflow: hidden;
}

.scrollArea { padding: 5px; }

.opt {
  padding: 0.75rem 1rem;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-bottom: 2px;
  border-left: 3px solid transparent;
}

.opt:hover:not(.optDisabled) {
  background: rgba(255, 255, 255, 0.6);
  transform: translateX(4px);
}

.optActive {
  background: color-mix(in srgb, var(--g-accent) 10%, white);
  border-left-color: var(--g-accent);
}

.optActive .optText {
  color: var(--g-accent);
  font-weight: 800;
}

.optContent {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.optText {
  font-weight: 600;
  color: #374151;
  transition: color 0.2s;
}

.checkBadge {
  font-size: 0.65rem;
  font-weight: 900;
  background: var(--g-accent);
  color: white;
  padding: 2px 6px;
  border-radius: 4px;
  letter-spacing: 0.5px;
}

.optDisabled { opacity: 0.4; cursor: not-allowed; }
</style>