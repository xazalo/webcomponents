<template>
  <div v-bind="$attrs" :class="$style.neuRoot" :style="neuStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen, $style[size]]" 
        @click="isOpen = !isOpen"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <div :class="$style.iconBox">
          <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round">
            <path d="m6 9 6 6 6-6"/>
          </svg>
        </div>
      </button>
      
      <Transition :name="$style.neuPop">
        <div v-if="isOpen" :class="$style.menu">
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
              <span :class="$style.itemLabel">{{ opt.label }}</span>
              <div v-if="modelValue === opt.value" :class="$style.checkInner" />
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
  color?: string;       /* Color de acento para iconos/texto seleccionado */
  // Personalización Neumórfica
  baseColor?: string;   /* Color de fondo (Crucial: debe ser el mismo que el del sitio) */
  radius?: string;      /* Redondez (ej: 12px o 50px para cápsula) */
  distance?: number;    /* Profundidad del relieve (4 a 12) */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'Seleccionar...',
  color: '#4f46e5',
  baseColor: '#e0e5ec',
  radius: '16px',
  distance: 6,
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

const neuStyles = computed(() => {
  const d = props.distance;
  const blur = d * 2;
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.color,
    '--n-radius': props.radius,
    '--n-shadow-dark': 'rgba(163, 177, 198, 0.8)',
    '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
    '--n-ext': `${d}px ${d}px ${blur}px var(--n-shadow-dark), -${d}px -${d}px ${blur}px var(--n-shadow-light)`,
    '--n-ins': `inset ${d/2}px ${d/2}px ${d}px var(--n-shadow-dark), inset -${d/2}px -${d/2}px ${d}px var(--n-shadow-light)`,
  };
});
</script>

<style module>
/* --- Animación Soft-Pop --- */
.neuPopEnterActive, .neuPopLeaveActive {
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.neuPopEnterFrom, .neuPopLeaveTo {
  opacity: 0;
  transform: translateY(-5px) scale(0.98);
}

.neuRoot { width: 100%; font-family: 'Inter', system-ui, sans-serif; }
.container { position: relative; }

/* --- Trigger (Efecto Extruido) --- */
.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: var(--n-bg);
  border: none;
  border-radius: var(--n-radius);
  color: #6d7c90;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: var(--n-ext);
}

.sm { padding: 0.5rem 1rem; font-size: 0.85rem; }
.md { padding: 0.8rem 1.4rem; font-size: 0.95rem; }
.lg { padding: 1.1rem 1.8rem; font-size: 1.1rem; }

.trigger:hover:not(:disabled) {
  color: var(--n-accent);
}

.triggerOpen {
  /* Al abrir, el botón pasa de extruido a hendido (inset) */
  box-shadow: var(--n-ins);
  transform: scale(0.985);
  color: var(--n-accent);
}

.iconBox {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
}

.icon { opacity: 0.6; transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1); }
.iconRotated { transform: rotate(180deg); opacity: 1; }

/* --- Dropdown Menu --- */
.menu {
  position: absolute;
  top: calc(100% + 18px);
  width: 100%;
  background: var(--n-bg);
  border-radius: var(--n-radius);
  padding: 0.7rem;
  z-index: 100;
  box-shadow: var(--n-ext);
}

.optionsList { display: flex; flex-direction: column; gap: 6px; }

.item {
  padding: 0.75rem 1rem;
  cursor: pointer;
  font-weight: 600;
  color: #6d7c90;
  border-radius: calc(var(--n-radius) * 0.7);
  transition: all 0.2s ease;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.item:hover:not(.itemDisabled) {
  background: rgba(255, 255, 255, 0.4);
  color: var(--n-accent);
}

.itemSelected {
  /* La opción seleccionada se "hunde" en la superficie del menú */
  color: var(--n-accent);
  box-shadow: var(--n-ins);
}

.checkInner {
  width: 8px; height: 8px;
  background: var(--n-accent);
  border-radius: 50%;
  box-shadow: 0 0 8px color-mix(in srgb, var(--n-accent) 40%, transparent);
}

.itemDisabled { opacity: 0.3; cursor: not-allowed; }
</style>