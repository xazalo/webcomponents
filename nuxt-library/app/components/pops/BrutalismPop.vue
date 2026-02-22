<template>
  <Teleport to="body">
    <Transition name="brutal-pop">
      <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
        <div v-bind="$attrs" :class="$style.brutalModal" :style="brutalStyles">
          <div :class="$style.brutalHeader">
            <span :class="$style.title">!! !! !!</span>
            <button @click="$emit('update:modelValue', false)" :class="$style.miniClose">×</button>
          </div>
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <button @click="$emit('update:modelValue', false)" :class="$style.mainAction">
              {{ buttonText }}
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
const props = defineProps<{ modelValue: boolean; color?: string, buttonText: string }>();

const brutalStyles = computed(() => ({
  '--b-accent': props.color || '#ff3e00', // Naranja vibrante
}));
</script>

<style module>
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(255, 255, 255, 0.8); /* Fondo claro, no borroso */
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
}

.brutalModal {
  background: #ffffff;
  width: 90%;
  max-width: 400px;
  border: 4px solid #000000;
  border-radius: 0px; /* Esquinas vivas */
  box-shadow: 12px 12px 0px 0px #000000;
  position: relative;
  overflow: hidden;
  animation: brutalSlide 0.2s cubic-bezier(0.18, 0.89, 0.32, 1.28);
}

.brutalHeader {
  background: #000000;
  padding: 0.5rem 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #ffffff;
}

.title {
  font-size: 0.75rem;
  font-weight: 900;
  letter-spacing: 0.1em;
}

.miniClose {
  background: var(--b-accent);
  border: 2px solid #ffffff;
  color: white;
  font-weight: 900;
  cursor: pointer;
  line-height: 1;
  padding: 2px 6px;
}

.inner { 
  padding: 2rem;
  display: flex; 
  flex-direction: column; 
  gap: 1.5rem; 
}

.content {
  font-family: 'Courier New', Courier, monospace; /* Tipografía tipo máquina */
  font-weight: 700;
  color: #000000;
  line-height: 1.4;
  font-size: 1.1rem;
}

.mainAction {
  background: var(--b-accent);
  color: #000000;
  border: 3px solid #000000;
  padding: 1rem;
  font-weight: 900;
  font-size: 1.2rem;
  text-transform: uppercase;
  cursor: pointer;
  box-shadow: 4px 4px 0px 0px #000000;
  transition: all 0.1s;
}

.mainAction:hover {
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0px 0px #000000;
}

.mainAction:active {
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px 0px #000000;
}

@keyframes brutalSlide {
  from { transform: scale(0.8) translateY(20px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}
</style>