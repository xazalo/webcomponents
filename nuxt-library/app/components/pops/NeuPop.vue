<template>
  <Teleport to="body">
    <Transition name="neu-fade">
      <div
        v-if="modelValue"
        :class="$style.overlay"
        @click.self="$emit('update:modelValue', false)"
      >
        <div v-bind="$attrs" :class="$style.neuModal" :style="neuStyles">
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>

            <button
              @click="$emit('update:modelValue', false)"
              :class="$style.closeButton"
            >
              {{ buttonText }}
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from "vue";
const props = defineProps<{ modelValue: boolean; color?: string, buttonText: string }>();

const neuStyles = computed(() => ({
  "--n-bg": "#e0e5ec",
  "--n-accent": props.color || "#4f46e5",
  "--n-shadow-dark": "rgba(163, 177, 198, 0.8)",
  "--n-shadow-light": "rgba(255, 255, 255, 0.9)",
}));
</script>

<style module>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(
    224,
    229,
    236,
    0.7
  ); /* Mismo tono que el modal pero con opacidad */
  backdrop-filter: blur(5px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.neuModal {
  background: var(--n-bg);
  width: 90%;
  max-width: 400px;
  padding: 3rem 2rem;
  border-radius: 30px;
  position: relative;

  /* RELIEVE NEUMÓRFICO CLÁSICO */
  box-shadow:
    20px 20px 60px var(--n-shadow-dark),
    -20px -20px 60px var(--n-shadow-light);

  border: 1px solid rgba(255, 255, 255, 0.2); /* Borde de luz muy fino */
}

.inner {
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
  align-items: center;
}

.content {
  font-size: 1.1rem;
  font-weight: 600;
  color: #6d7c90;
  line-height: 1.6;
  text-align: center;
}

.closeButton {
  background: var(--n-bg);
  color: var(--n-accent);
  border: none;
  padding: 0.8rem 2.5rem;
  font-size: 1rem;
  font-weight: 700;
  border-radius: 15px;
  cursor: pointer;
  transition: all 0.3s ease;

  /* Botón con relieve inverso al presionar */
  box-shadow:
    6px 6px 12px var(--n-shadow-dark),
    -6px -6px 12px var(--n-shadow-light);
}

.closeButton:hover {
  transform: translateY(-2px);
  color: #312e81;
}

.closeButton:active {
  transform: translateY(0);
  box-shadow:
    inset 4px 4px 8px var(--n-shadow-dark),
    inset -4px -4px 8px var(--n-shadow-light);
}

/* Animación de entrada sutil */
:global(.neu-fade-enter-active) {
  animation: neuIn 0.4s ease-out;
}
:global(.neu-fade-leave-active) {
  transition: opacity 0.3s ease;
}

@keyframes neuIn {
  from {
    transform: scale(0.9);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
