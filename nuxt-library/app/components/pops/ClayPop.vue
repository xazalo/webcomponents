<template>
  <Teleport to="body">
    <Transition name="clay-pop">
      <div v-if="modelValue" :class="$style.overlay" @click.self="$emit('update:modelValue', false)">
        <div v-bind="$attrs" :class="$style.clayModal" :style="clayStyles">
          <div :class="$style.decorLight"></div>
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <button @click="$emit('update:modelValue', false)" :class="$style.closeButton">
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

const clayStyles = computed(() => ({
  '--c-accent': props.color || '#ff7eb9', // Rosa "bubblegum"
  '--c-bg': '#ffffff'
}));
</script>

<style module>
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(240, 244, 248, 0.6); 
  backdrop-filter: blur(10px);
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
}

.clayModal {
  background: var(--c-bg);
  width: 90%;
  max-width: 400px;
  padding: 2.5rem;
  position: relative;
  
  /* Radios orgánicos y muy amplios */
  border-radius: 40px;
  
  /* EL EFECTO CLAY: Sombra externa suave + Sombras internas (Luz y Profundidad) */
  box-shadow: 
    0 20px 40px rgba(0, 0, 0, 0.1),
    inset 0 10px 20px rgba(255, 255, 255, 1),
    inset 0 -10px 20px rgba(0, 0, 0, 0.05);
  
  animation: clayBounce 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.decorLight {
  position: absolute;
  top: 10%;
  left: 10%;
  width: 60px;
  height: 30px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  filter: blur(8px);
  transform: rotate(-15deg);
  pointer-events: none;
}

.inner { 
  display: flex; 
  flex-direction: column; 
  gap: 2rem; 
  align-items: center;
}

.content {
  font-size: 1.1rem;
  font-weight: 700;
  color: #475569;
  line-height: 1.5;
  text-align: center;
}

.closeButton {
  background: var(--c-accent);
  color: white;
  border: none;
  padding: 1rem 2rem;
  font-size: 1.1rem;
  font-weight: 800;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  
  /* Botón estilo Clay también */
  box-shadow: 
    0 10px 20px rgba(0, 0, 0, 0.1),
    inset 0 4px 8px rgba(255, 255, 255, 0.4),
    inset 0 -4px 8px rgba(0, 0, 0, 0.2);
}

.closeButton:hover {
  transform: scale(1.05) translateY(-2px);
  filter: brightness(1.05);
}

.closeButton:active {
  transform: scale(0.95);
}

@keyframes clayBounce {
  0% { transform: scale(0.5); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

/* Transiciones de Vue */
:global(.clay-pop-enter-active), :global(.clay-pop-leave-active) {
  transition: opacity 0.3s ease;
}
:global(.clay-pop-enter-from), :global(.clay-pop-leave-to) {
  opacity: 0;
}
</style>