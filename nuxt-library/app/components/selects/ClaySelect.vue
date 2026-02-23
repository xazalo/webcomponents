<template>
  <div v-bind="$attrs" :class="$style.clayRoot" :style="clayStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
        :disabled="disabled"
      >
        <div v-if="highLight" :class="$style.decorLight"></div>
        
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition :name="$style.clayAnim">
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
              <div v-if="modelValue === opt.value" :class="$style.itemDot" />
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
  color?: string;       /* Color cuando está activo o seleccionado */
  // Parámetros Clay
  radius?: string;      /* Nivel de redondez (ej: 25px) */
  depth?: number;       /* Intensidad de sombras internas (0-10) */
  highLight?: boolean;  /* ¿Pequeño brillo tipo plástico? */
  size?: 'sm' | 'md' | 'lg';
  menuAlign?: 'left' | 'right';
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Elegir...',
  color: '#38bdf8',
  radius: '20px',
  depth: 5,
  highLight: true,
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

const clayStyles = computed(() => ({
  '--c-accent': props.color,
  '--c-radius': props.radius,
  '--c-inner-light': `inset 0 ${props.depth}px ${props.depth * 2}px rgba(255, 255, 255, 0.8)`,
  '--c-inner-dark': `inset 0 -${props.depth}px ${props.depth * 2}px rgba(0, 0, 0, 0.05)`,
  '--c-shadow-ext': `0 ${props.depth * 2}px ${props.depth * 4}px -${props.depth}px rgba(0, 0, 0, 0.1)`,
}));
</script>

<style module>
/* --- Animación Elástica --- */
.clayAnimEnterActive {
  animation: clayPop 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.clayAnimLeaveActive {
  transition: all 0.2s ease-in;
  opacity: 0;
  transform: scale(0.95);
}

@keyframes clayPop {
  0% { transform: scale(0.8) translateY(-10px); opacity: 0; }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}

.clayRoot { width: 100%; font-family: 'Inter', system-ui, sans-serif; }
.container { position: relative; }

/* --- Trigger --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: none;
  border-radius: var(--c-radius);
  color: #475569;
  font-weight: 700;
  cursor: pointer;
  position: relative;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: var(--c-shadow-ext), var(--c-inner-light), var(--c-inner-dark);
}

.sm { padding: 0.5rem 1rem; font-size: 0.85rem; }
.md { padding: 0.8rem 1.5rem; font-size: 1rem; }
.lg { padding: 1.1rem 2rem; font-size: 1.15rem; }

.decorLight {
  position: absolute; top: 15%; left: 8%; width: 30px; height: 10px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 50%; filter: blur(4px); transform: rotate(-10deg);
  pointer-events: none;
}

.trigger:hover:not(:disabled) {
  transform: translateY(-3px);
  filter: brightness(1.02);
}

.triggerOpen {
  background: var(--c-accent);
  color: white;
  transform: scale(0.96);
  box-shadow: 
    inset 0 4px 10px rgba(0, 0, 0, 0.1),
    inset 0 -4px 10px rgba(255, 255, 255, 0.2);
}

.label { z-index: 2; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.icon { transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1); opacity: 0.5; }
.triggerOpen .icon { transform: rotate(180deg); opacity: 1; color: white; }

/* --- Dropdown Menu --- */
.menu {
  position: absolute;
  top: calc(100% + 12px);
  min-width: 100%;
  background: #ffffff;
  border-radius: calc(var(--c-radius) * 1.2);
  padding: 0.6rem;
  z-index: 100;
  box-shadow: 
    0 25px 50px -12px rgba(0, 0, 0, 0.15),
    var(--c-inner-light);
}

.align-left { left: 0; }
.align-right { right: 0; }

.optionsList { display: flex; flex-direction: column; gap: 4px; }

.item {
  padding: 0.75rem 1.2rem;
  cursor: pointer;
  font-weight: 600;
  color: #64748b;
  border-radius: calc(var(--c-radius) * 0.8);
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.2s ease;
}

.item:hover:not(.itemDisabled) {
  background: color-mix(in srgb, var(--c-accent) 8%, white);
  color: var(--c-accent);
  transform: translateX(5px);
}

.itemSelected {
  background: var(--c-accent);
  color: white;
  box-shadow: 
    inset 0 2px 5px rgba(255, 255, 255, 0.3),
    0 5px 15px color-mix(in srgb, var(--c-accent) 30%, transparent);
}

.itemDot {
  width: 8px; height: 8px; background: white; border-radius: 50%;
}

.itemDisabled { opacity: 0.4; cursor: not-allowed; }
</style>