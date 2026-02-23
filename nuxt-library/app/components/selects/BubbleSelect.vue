<template>
  <div v-bind="$attrs" :class="$style.bubbleRoot" :style="bubbleStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
        :disabled="disabled"
      >
        <div v-if="glossy" :class="$style.gloss" />
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition :name="$style.bubbleAnim">
        <div v-if="isOpen" :class="[$style.menu, $style[`align-${menuAlign}`]]">
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
              <span :class="$style.itemText">{{ opt.label }}</span>
              <div v-if="modelValue === opt.value" :class="$style.bubbleCheck" />
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
  color?: string;
  // Configuración Bubble
  irregularity?: number; /* Grado de "deformidad" (1-10) */
  size?: 'sm' | 'md' | 'lg';
  glossy?: boolean;      /* Brillo tipo caramelo */
  bounce?: boolean;      /* Animación elástica */
  menuAlign?: 'left' | 'right';
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Elige uno...',
  color: '#f472b6',
  irregularity: 4,
  size: 'md',
  glossy: true,
  bounce: true,
  menuAlign: 'left',
});

const emit = defineEmits(['update:modelValue', 'change']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);

const selectOption = (v: any) => {
  emit('update:modelValue', v);
  emit('change', v);
  isOpen.value = false;
};

const bubbleStyles = computed(() => {
  const irr = props.irregularity;
  // Radio orgánico para el trigger
  const br = `${50 + irr}% ${50 - irr}% ${60 + irr}% ${40 - irr}% / ${45 - irr}% ${55 + irr}% ${45 - irr}% ${55 + irr}%`;
  
  return {
    '--b-accent': props.color,
    '--b-radius': br,
    '--b-anim': props.bounce ? 'bubbleBounce 0.5s cubic-bezier(0.34, 1.56, 0.64, 1)' : 'simpleFade 0.2s ease',
  };
});
</script>

<style module>
/* --- Animación Bubble Pop --- */
.bubbleAnimEnterActive { animation: var(--b-anim); }
.bubbleAnimLeaveActive { animation: var(--b-anim) reverse; }

@keyframes bubbleBounce {
  0% { transform: scale(0.4) translateY(-20px); opacity: 0; }
  70% { transform: scale(1.1); }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}

.bubbleRoot { width: 100%; font-family: 'Quicksand', sans-serif; }
.container { position: relative; }

/* --- Trigger --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: 4px solid var(--b-accent);
  border-radius: var(--b-radius);
  color: #1f2937;
  font-weight: 800;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: 0 8px 20px -5px rgba(0, 0, 0, 0.1);
}

.sm { padding: 0.5rem 1rem; font-size: 0.85rem; }
.md { padding: 0.8rem 1.5rem; font-size: 1rem; }
.lg { padding: 1.1rem 2rem; font-size: 1.2rem; }

.trigger:hover:not(:disabled) {
  transform: scale(1.02) translateY(-2px);
  box-shadow: 0 12px 25px -5px rgba(0, 0, 0, 0.15);
}

.triggerOpen {
  border-color: #1f2937;
  transform: scale(0.98);
}

.label { z-index: 2; position: relative; }

/* --- Brillo Decorativo --- */
.gloss {
  position: absolute;
  top: 15%; left: 10%; width: 25%; height: 20%;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50%; filter: blur(4px);
  pointer-events: none;
}

.icon { 
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  color: var(--b-accent);
}
.iconRotated { transform: rotate(180deg) scale(1.2); }

/* --- Dropdown Menu --- */
.menu {
  position: absolute;
  top: 110%;
  min-width: 100%;
  background: #ffffff;
  border: 4px solid #1f2937;
  border-radius: 30px;
  padding: 0.6rem;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
  z-index: 100;
}

.align-left { left: 0; }
.align-right { right: 0; }

.optionsList { display: flex; flex-direction: column; gap: 4px; }

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 1rem;
  border-radius: 20px;
  cursor: pointer;
  font-weight: 700;
  color: #4b5563;
  transition: all 0.2s ease;
}

.item:hover:not(.itemDisabled) {
  background: color-mix(in srgb, var(--b-accent) 15%, transparent);
  color: var(--b-accent);
  transform: translateX(5px);
}

.itemSelected {
  background: var(--b-accent);
  color: #ffffff;
}

.bubbleCheck {
  width: 10px; height: 10px;
  background: white; border-radius: 50%;
  box-shadow: 0 0 10px rgba(255,255,255,0.8);
}

.itemDisabled { opacity: 0.4; cursor: not-allowed; }
</style>