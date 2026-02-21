<template>
  <div v-bind="$attrs" :class="$style.glassRoot" :style="glassStyles">
    <div :class="$style.wrapper" @click="isOpen = !isOpen">
      <span :class="$style.val">{{ selectedLabel || placeholder }}</span>
      <Icon name="lucide:chevrons-up-down" :class="$style.icon" />
      
      <div v-if="isOpen" :class="$style.dropdown">
        <div 
          v-for="opt in options" 
          :key="opt.value" 
          :class="$style.opt"
          @click.stop="selectOption(opt.value)"
        >
          {{ opt.label }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
const props = defineProps<{ modelValue: any; options: any[]; placeholder?: string; shadow?: string | boolean }>();
const emit = defineEmits(['update:modelValue']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);
const selectOption = (v: any) => { emit('update:modelValue', v); isOpen.value = false; };

const glassStyles = computed(() => ({
  '--g-shadow': props.shadow === true ? "0 8px 32px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.glassRoot { width: 100%; }

.wrapper {
  position: relative;
  padding: 0.7rem 1.2rem;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 14px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  box-shadow: var(--g-shadow);
}

.dropdown {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(20px);
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  overflow: hidden;
  z-index: 100;
}

.opt {
  padding: 0.7rem 1.2rem;
  transition: background 0.2s;
  font-weight: 500;
}

.opt:hover { background: rgba(255, 255, 255, 0.5); }
.icon { width: 1rem; opacity: 0.5; }
</style>