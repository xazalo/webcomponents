<template>
  <div v-bind="$attrs" :class="$style.bentoRoot" :style="bentoStyles">
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, isOpen && $style.triggerOpen]" 
        @click="isOpen = !isOpen"
      >
        <span :class="$style.label">{{ selectedLabel || placeholder }}</span>
        <svg :class="[$style.icon, isOpen && $style.iconRotated]" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </button>
      
      <Transition name="bento-menu">
        <div v-if="isOpen" :class="$style.menu">
          <div 
            v-for="opt in options" 
            :key="opt.value" 
            :class="[$style.item, modelValue === opt.value && $style.itemSelected]"
            @click="selectOption(opt.value)"
          >
            {{ opt.label }}
            <div v-if="modelValue === opt.value" :class="$style.checkDot"></div>
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

const bentoStyles = computed(() => ({
  '--b-accent': props.color || '#4f46e5',
}));
</script>

<style module>
.bentoRoot { width: 100%; font-family: system-ui, -apple-system, sans-serif; }
.container { position: relative; }

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 1rem;
  background: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.08);
  border-radius: 0.8rem;
  font-weight: 600;
  font-size: 0.9rem;
  color: #1f2937;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.trigger:hover {
  background: #f9fafb;
  border-color: rgba(0, 0, 0, 0.15);
}

.triggerOpen {
  border-color: var(--b-accent);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--b-accent) 15%, transparent);
}

.label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.icon { opacity: 0.4; transition: transform 0.3s ease; }
.iconRotated { transform: rotate(180deg); opacity: 1; color: var(--b-accent); }

.menu {
  position: absolute;
  top: calc(100% + 6px);
  width: 100%;
  background: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.08);
  border-radius: 0.8rem;
  padding: 0.4rem;
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.05);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.6rem 0.75rem;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: 500;
  font-size: 0.85rem;
  color: #4b5563;
  transition: all 0.15s ease;
}

.item:hover {
  background: #f3f4f6;
  color: #111827;
}

.itemSelected {
  background: color-mix(in srgb, var(--b-accent) 10%, white);
  color: var(--b-accent);
  font-weight: 600;
}

.checkDot {
  width: 6px;
  height: 6px;
  background: var(--b-accent);
  border-radius: 50%;
}

/* Animación de apertura estilo Bento */
:global(.bento-menu-enter-active) {
  transition: all 0.2s ease-out;
}
:global(.bento-menu-enter-from) {
  opacity: 0;
  transform: translateY(-8px) scale(0.98);
}
</style>