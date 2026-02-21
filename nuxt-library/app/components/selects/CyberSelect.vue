<template>
  <div v-bind="$attrs" :class="$style.selectRoot" :style="selectStyles">
    <label v-if="label" :class="$style.label">{{ label }}</label>
    <div :class="$style.container" v-click-outside="() => isOpen = false">
      <button 
        type="button"
        :class="[$style.trigger, { [$style.isOpen]: isOpen }]" 
        @click="isOpen = !isOpen"
      >
        <span>{{ selectedLabel || placeholder }}</span>
        <Icon name="lucide:chevron-down" :class="[$style.chevron, { [$style.rotate]: isOpen }]" />
      </button>

      <Transition name="fade">
        <ul v-if="isOpen" :class="$style.options">
          <li 
            v-for="opt in options" 
            :key="opt.value" 
            :class="[$style.option, { [$style.active]: modelValue === opt.value }]"
            @click="selectOption(opt.value)"
          >
            {{ opt.label }}
          </li>
        </ul>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Option { label: string; value: any; }

const props = defineProps<{
  modelValue: any;
  options: Option[];
  label?: string;
  placeholder?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const emit = defineEmits(['update:modelValue']);
const isOpen = ref(false);

const selectedLabel = computed(() => 
  props.options.find(o => o.value === props.modelValue)?.label
);

const selectStyles = computed(() => ({
  '--s-color': props.color || '#4f46e5',
  '--s-shadow': props.shadow === true ? "6px 6px 0px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));

const selectOption = (val: any) => {
  emit('update:modelValue', val);
  isOpen.value = false;
};
</script>

<style module>
.selectRoot { display: flex; flex-direction: column; gap: 0.5rem; width: 100%; }
.label { font-size: 0.7rem; font-weight: 900; text-transform: uppercase; color: #374151; }
.container { position: relative; }

.trigger {
  width: 100%;
  padding: 0.8rem 1.2rem;
  background: #111827;
  color: white;
  border: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  clip-path: polygon(10px 0%, 100% 0%, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0% 100%, 0% 10px);
  box-shadow: var(--s-shadow);
  font-weight: 600;
}

.options {
  position: absolute;
  top: 110%;
  left: 0;
  width: 100%;
  background: #1f2937;
  list-style: none;
  padding: 0;
  margin: 0;
  z-index: 100;
  clip-path: polygon(0 0, 100% 0, 100% 90%, 90% 100%, 0 100%);
  border-left: 2px solid var(--s-color);
}

.option {
  padding: 0.75rem 1.2rem;
  color: #9ca3af;
  cursor: pointer;
  transition: all 0.2s;
}

.option:hover, .active {
  background: var(--s-color);
  color: white;
}

.chevron { transition: transform 0.3s; }
.rotate { transform: rotate(180deg); }
</style>