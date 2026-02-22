<template>
  <div v-bind="$attrs" :class="$style.brutalRoot" :style="brutalStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen]" 
        @click="isOpen = !isOpen"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="square">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
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

const brutalStyles = computed(() => ({
  '--b-accent': props.color || '#bc95ff', // Púrpura brutalista por defecto
}));
</script>

<style module>
.brutalRoot { width: 100%; }
.container { position: relative; }

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.8rem 1.2rem;
  background: #ffffff;
  border: 3px solid #000000;
  border-radius: 4px; /* Casi cuadrado */
  font-weight: 800;
  font-size: 1rem;
  color: #000000;
  text-transform: uppercase;
  cursor: pointer;
  box-shadow: 4px 4px 0px 0px #000000;
  transition: all 0.1s ease;
}

.trigger:hover {
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0px 0px #000000;
  background: var(--b-accent);
}

.triggerOpen {
  background: var(--b-accent);
  transform: translate(2px, 2px);
  box-shadow: 2px 2px 0px 0px #000000;
}

.label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.icon { transition: transform 0.2s steps(4); }
.iconRotated { transform: rotate(180deg); }

.menu {
  position: absolute;
  top: calc(100% + 8px);
  width: 100%;
  background: #ffffff;
  border: 3px solid #000000;
  border-radius: 4px;
  padding: 0;
  box-shadow: 6px 6px 0px 0px #000000;
  z-index: 100;
  overflow: hidden;
}

.item {
  padding: 0.8rem 1.2rem;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.9rem;
  color: #000000;
  border-bottom: 2px solid #000000;
  transition: background 0.1s;
}

.item:last-child { border-bottom: none; }

.item:hover {
  background: #000000;
  color: #ffffff;
}

.itemSelected {
  background: var(--b-accent);
  text-decoration: underline;
  text-decoration-thickness: 3px;
}
</style>