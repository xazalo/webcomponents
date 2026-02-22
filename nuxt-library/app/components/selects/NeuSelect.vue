<template>
  <div v-bind="$attrs" :class="$style.neuRoot" :style="neuStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen]" 
        @click="isOpen = !isOpen"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition name="neu-slide">
        <div v-if="isOpen" :class="$style.menu">
          <div 
            v-for="opt in options" 
            :key="opt.value" 
            :class="[$style.item, modelValue === opt.value && $style.itemSelected]"
            @click="selectOption(opt.value)"
          >
            {{ opt.label }}
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

const neuStyles = computed(() => ({
  '--n-bg': '#e0e5ec',
  '--n-accent': props.color || '#4f46e5',
  '--n-shadow-dark': 'rgba(163, 177, 198, 0.8)',
  '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
}));
</script>

<style module>
.neuRoot { width: 100%; font-family: sans-serif; }
.container { position: relative; }

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.8rem 1.2rem;
  background: var(--n-bg);
  border: none;
  border-radius: 12px;
  font-weight: 600;
  color: #6d7c90;
  cursor: pointer;
  transition: all 0.3s ease;
  
  /* Elevación Neumórfica */
  box-shadow: 
    6px 6px 12px var(--n-shadow-dark),
    -6px -6px 12px var(--n-shadow-light);
}

.trigger:hover {
  color: var(--n-accent);
}

.triggerOpen {
  /* Al abrir, el botón se "hunde" ligeramente */
  box-shadow: 
    inset 4px 4px 8px var(--n-shadow-dark),
    inset -4px -4px 8px var(--n-shadow-light);
  transform: scale(0.99);
}

.label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.icon { opacity: 0.6; transition: transform 0.3s ease; }
.iconRotated { transform: rotate(180deg); color: var(--n-accent); }

.menu {
  position: absolute;
  top: calc(100% + 15px);
  width: 100%;
  background: var(--n-bg);
  border-radius: 12px;
  padding: 0.5rem;
  z-index: 100;
  
  /* Elevación del menú */
  box-shadow: 
    10px 10px 20px var(--n-shadow-dark),
    -10px -10px 20px var(--n-shadow-light);
}

.item {
  padding: 0.7rem 1rem;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  color: #6d7c90;
  border-radius: 8px;
  transition: all 0.2s ease;
  margin-bottom: 2px;
}

.item:hover {
  background: rgba(255, 255, 255, 0.3);
  color: var(--n-accent);
}

.itemSelected {
  /* La opción seleccionada parece hendida en el menú */
  color: var(--n-accent);
  box-shadow: 
    inset 3px 3px 6px var(--n-shadow-dark),
    inset -3px -3px 6px var(--n-shadow-light);
}

/* Transición suave */
:global(.neu-slide-enter-active), :global(.neu-slide-leave-active) {
  transition: all 0.3s ease;
}
:global(.neu-slide-enter-from), :global(.neu-slide-leave-to) {
  opacity: 0;
  transform: translateY(-10px);
}
</style>