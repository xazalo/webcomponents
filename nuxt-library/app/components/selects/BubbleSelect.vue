<template>
  <div v-bind="$attrs" :class="$style.bubbleRoot" :style="bubbleStyles">
    <div :class="$style.container">
      <button :class="$style.trigger" @click="isOpen = !isOpen">
        {{ selectedLabel || placeholder }}
      </button>
      
      <div v-if="isOpen" :class="$style.menu">
        <div 
          v-for="opt in options" 
          :key="opt.value" 
          :class="$style.item"
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
const props = defineProps<{ modelValue: any; options: any[]; placeholder?: string; color?: string; shadow?: string | boolean }>();
const emit = defineEmits(['update:modelValue']);
const isOpen = ref(false);

const selectedLabel = computed(() => props.options.find(o => o.value === props.modelValue)?.label);
const selectOption = (v: any) => { emit('update:modelValue', v); isOpen.value = false; };

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#f472b6',
  '--b-shadow': props.shadow === true ? "0 10px 25px -5px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.bubbleRoot { width: 100%; }
.container { position: relative; }

.trigger {
  width: 100%;
  padding: 0.8rem 1.5rem;
  background: white;
  border: 3px solid var(--b-color);
  border-radius: 60% 40% 70% 30% / 40% 50% 60% 40%;
  font-weight: 700;
  cursor: pointer;
  box-shadow: var(--b-shadow);
  transition: all 0.3s;
}

.menu {
  position: absolute;
  top: 105%;
  width: 100%;
  background: white;
  border-radius: 20px;
  padding: 0.5rem;
  box-shadow: var(--b-shadow);
  z-index: 50;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.item {
  padding: 0.6rem 1rem;
  border-radius: 12px;
  cursor: pointer;
  font-weight: 600;
}

.item:hover { background: #fdf2f8; color: var(--b-color); }
</style>