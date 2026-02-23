<template>
  <div v-bind="$attrs" :class="$style.brutalRoot" :style="brutalStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[
          $style.trigger, 
          isOpen && $style.triggerOpen,
          $style[size]
        ]" 
        @click="isOpen = !isOpen"
        :disabled="disabled"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <div :class="$style.iconBox">
          <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="square">
            <path d="m6 9 6 6 6-6"/>
          </svg>
        </div>
      </button>
      
      <Transition :name="$style.brutalSlide">
        <div v-if="isOpen" :class="[$style.menu, $style[`align-${menuAlign}`]]">
          <div v-if="menuTitle" :class="$style.menuHeader">{{ menuTitle }}</div>
          
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
              <div v-if="modelValue === opt.value" :class="$style.checkBlock">✓</div>
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

interface Option {
  label: string;
  value: any;
  disabled?: boolean;
}

interface Props {
  modelValue: any;
  options: Option[];
  placeholder?: string;
  color?: string;       /* Color Neobrutalista vibrante */
  // Configuración Brutal
  borderWidth?: string; /* Grosor de los bordes (ej: 3px) */
  shadowSize?: string;  /* Tamaño de la sombra dura (ej: 6px) */
  size?: 'sm' | 'md' | 'lg';
  menuAlign?: 'left' | 'right';
  menuTitle?: string;
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'SELECT_OPTION',
  color: '#bc95ff',
  borderWidth: '3px',
  shadowSize: '6px',
  size: 'md',
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

const brutalStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-border': props.borderWidth,
  '--b-shadow': props.shadowSize,
}));
</script>

<style module>
/* --- Animación "Choppy" (Toscas) --- */
.brutalSlideEnterActive {
  transition: all 0.15s steps(3);
}
.brutalSlideEnterFrom {
  opacity: 0;
  transform: translate(var(--b-shadow), var(--b-shadow));
}

.brutalRoot { width: 100%; font-family: 'Archivo Black', sans-serif, system-ui; }
.container { position: relative; }

/* --- Trigger --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: var(--b-border) solid #000000;
  color: #000000;
  font-weight: 800;
  text-transform: uppercase;
  cursor: pointer;
  box-shadow: var(--b-shadow) var(--b-shadow) 0px 0px #000000;
  transition: all 0.1s;
  border-radius: 2px;
}

.sm { padding: 0.5rem 0.8rem; font-size: 0.8rem; }
.md { padding: 0.8rem 1.2rem; font-size: 1rem; }
.lg { padding: 1.2rem 1.5rem; font-size: 1.2rem; }

.trigger:hover:not(:disabled) {
  transform: translate(-2px, -2px);
  box-shadow: calc(var(--b-shadow) + 2px) calc(var(--b-shadow) + 2px) 0px 0px #000000;
  background: var(--b-accent);
}

.triggerOpen {
  background: var(--b-accent);
  transform: translate(2px, 2px);
  box-shadow: 2px 2px 0px 0px #000000;
}

.trigger:disabled { cursor: not-allowed; opacity: 0.5; background: #eee; }

.label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.iconBox {
  display: flex;
  align-items: center;
  padding-left: 10px;
  border-left: var(--b-border) solid #000;
  margin-left: 10px;
}

.icon { transition: transform 0.2s steps(4); }
.iconRotated { transform: rotate(180deg); }

/* --- Menu Dropdown --- */
.menu {
  position: absolute;
  top: calc(100% + 12px);
  width: 100%;
  background: #ffffff;
  border: var(--b-border) solid #000000;
  box-shadow: var(--b-shadow) var(--b-shadow) 0px 0px #000000;
  z-index: 100;
}

.align-left { left: 0; }
.align-right { right: 0; }

.menuHeader {
  background: #000;
  color: #fff;
  padding: 0.4rem 0.8rem;
  font-size: 0.7rem;
  font-weight: 900;
}

.optionsList { display: flex; flex-direction: column; }

.item {
  padding: 0.8rem 1.2rem;
  cursor: pointer;
  font-weight: 700;
  color: #000000;
  border-bottom: var(--b-border) solid #000000;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.item:last-child { border-bottom: none; }

.item:hover:not(.itemDisabled) {
  background: #000000;
  color: #ffffff;
}

.itemSelected {
  background: var(--b-accent);
  color: #000;
}

.checkBlock {
  background: #000;
  color: #fff;
  padding: 2px 6px;
  font-size: 0.8rem;
}

.itemSelected:hover .checkBlock {
  background: #fff;
  color: #000;
}

.itemDisabled { opacity: 0.3; cursor: not-allowed; background: #f0f0f0; }
</style>