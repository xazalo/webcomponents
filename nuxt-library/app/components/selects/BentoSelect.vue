<template>
  <div v-bind="$attrs" :class="$style.bentoRoot" :style="bentoStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
        :disabled="disabled"
      >
        <div :class="$style.triggerContent">
          <div v-if="showDecor" :class="$style.decorDot" />
          <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        </div>
        
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition :name="$style.bentoMenuAnim">
        <div v-if="isOpen" :class="[$style.menu, $style[`align-${menuAlign}`]]">
          <header v-if="menuTitle" :class="$style.menuHeader">{{ menuTitle }}</header>
          
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
              <div :class="$style.itemLabel">
                {{ opt.label }}
              </div>
              <div v-if="modelValue === opt.value" :class="$style.checkMark">
                <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
                  <path d="M20 6 9 17l-5-5"/>
                </svg>
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

interface Option {
  label: string;
  value: any;
  disabled?: boolean;
}

interface Props {
  modelValue: any;
  options: Option[];
  placeholder?: string;
  color?: string;
  // Configuración Bento
  radius?: string;        /* Radio personalizado (ej: 1rem) */
  size?: 'sm' | 'md' | 'lg';
  menuAlign?: 'left' | 'right';
  menuTitle?: string;     /* Título pequeño dentro del dropdown */
  showDecor?: boolean;    /* El punto decorativo Bento en el trigger */
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Seleccionar...',
  color: '#4f46e5',
  radius: '0.85rem',
  size: 'md',
  menuAlign: 'left',
  showDecor: true
});

const emit = defineEmits(['update:modelValue', 'change']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);

const selectOption = (v: any) => {
  emit('update:modelValue', v);
  emit('change', v);
  isOpen.value = false;
};

const bentoStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-radius': props.radius,
  '--b-accent-soft': `color-mix(in srgb, ${props.color} 10%, white)`,
}));
</script>

<style module>
/* --- Animación Bento --- */
.bentoMenuAnimEnterActive, .bentoMenuAnimLeaveActive {
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}
.bentoMenuAnimEnterFrom, .bentoMenuAnimLeaveTo {
  opacity: 0;
  transform: translateY(-10px) scale(0.95);
}

.bentoRoot { width: 100%; font-family: system-ui, -apple-system, sans-serif; }
.container { position: relative; }

/* --- Trigger (El botón) --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.08);
  border-radius: var(--b-radius);
  color: #1f2937;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.03);
}

.sm { padding: 0.5rem 0.75rem; font-size: 0.8rem; }
.md { padding: 0.7rem 1rem; font-size: 0.9rem; }
.lg { padding: 1rem 1.25rem; font-size: 1rem; }

.triggerContent { display: flex; align-items: center; gap: 10px; overflow: hidden; }

.decorDot {
  width: 8px; height: 8px; border-radius: 2px;
  background: var(--b-accent); opacity: 0.5;
  flex-shrink: 0;
}

.trigger:hover:not(:disabled) {
  background: #fdfdfd;
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
}

.triggerOpen {
  border-color: var(--b-accent);
  background: #ffffff;
}

.trigger:disabled { cursor: not-allowed; opacity: 0.6; background: #f3f4f6; }

.label { 
  font-weight: 600; 
  overflow: hidden; 
  text-overflow: ellipsis; 
  white-space: nowrap; 
}

.icon { opacity: 0.3; transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); }
.iconRotated { transform: rotate(180deg); opacity: 1; color: var(--b-accent); }

/* --- Dropdown Menu --- */
.menu {
  position: absolute;
  top: calc(100% + 8px);
  min-width: 100%;
  background: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: var(--b-radius);
  padding: 0.5rem;
  box-shadow: 
    0 20px 25px -5px rgba(0, 0, 0, 0.1), 
    0 0 0 1px rgba(0, 0, 0, 0.05);
  z-index: 100;
}

.align-left { left: 0; }
.align-right { right: 0; }

.menuHeader {
  padding: 0.5rem 0.75rem;
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: #9ca3af;
  font-weight: 700;
}

.optionsList { display: flex; flex-direction: column; gap: 2px; }

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.65rem 0.75rem;
  border-radius: calc(var(--b-radius) * 0.7);
  cursor: pointer;
  transition: all 0.1s ease;
}

.itemLabel { font-size: 0.85rem; font-weight: 500; color: #4b5563; }

.item:hover:not(.itemDisabled) {
  background: #f9fafb;
}

.itemSelected {
  background: var(--b-accent-soft);
}

.itemSelected .itemLabel { color: var(--b-accent); font-weight: 700; }

.checkMark { color: var(--b-accent); display: flex; align-items: center; }

.itemDisabled { opacity: 0.4; cursor: not-allowed; }
</style>