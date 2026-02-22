<template>
  <div v-bind="$attrs" :class="$style.clayRoot" :style="clayStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen]" 
        @click="isOpen = !isOpen"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition name="clay-drop">
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

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#38bdf8', // Celeste "baby blue"
}));
</script>

<style module>
.clayRoot { width: 100%; }
.container { position: relative; }

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.8rem 1.5rem;
  background: #ffffff;
  border: none;
  border-radius: 20px;
  font-weight: 700;
  font-size: 1rem;
  color: #475569;
  cursor: pointer;
  
  /* Efecto Claymorphism */
  box-shadow: 
    0 10px 20px -5px rgba(0, 0, 0, 0.05),
    inset 0 4px 6px rgba(255, 255, 255, 0.8),
    inset 0 -4px 10px rgba(0, 0, 0, 0.03);
    
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.trigger:hover {
  transform: translateY(-2px);
  box-shadow: 
    0 15px 25px -8px rgba(0, 0, 0, 0.08),
    inset 0 4px 6px rgba(255, 255, 255, 0.9),
    inset 0 -4px 10px rgba(0, 0, 0, 0.04);
}

.triggerOpen {
  background: var(--c-accent);
  color: white;
  transform: scale(0.98);
  box-shadow: 
    0 5px 10px rgba(0, 0, 0, 0.05),
    inset 0 4px 8px rgba(255, 255, 255, 0.3),
    inset 0 -4px 8px rgba(0, 0, 0, 0.2);
}

.label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.icon { transition: transform 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55); opacity: 0.6; }
.triggerOpen .icon { transform: rotate(180deg); opacity: 1; }

.menu {
  position: absolute;
  top: calc(100% + 12px);
  width: 100%;
  background: #ffffff;
  border-radius: 25px;
  padding: 0.6rem;
  z-index: 100;
  
  /* Menu con volumen */
  box-shadow: 
    0 20px 40px -10px rgba(0, 0, 0, 0.1),
    inset 0 4px 10px rgba(255, 255, 255, 0.8);
}

.item {
  padding: 0.7rem 1.2rem;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  color: #64748b;
  border-radius: 15px;
  transition: all 0.2s ease;
}

.item:hover {
  background: color-mix(in srgb, var(--c-accent) 10%, white);
  color: var(--c-accent);
  transform: translateX(4px);
}

.itemSelected {
  background: var(--c-accent);
  color: white;
  box-shadow: inset 0 2px 4px rgba(255, 255, 255, 0.4);
}

/* Animación de apertura elástica */
:global(.clay-drop-enter-active) {
  animation: clayIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

@keyframes clayIn {
  0% { transform: translateY(-10px) scale(0.9); opacity: 0; }
  100% { transform: translateY(0) scale(1); opacity: 1; }
}
</style>