<template>
  <Teleport to="body">
    <Transition :name="$style.neuFade">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.neuModal, $style[size]]" 
          :style="neuStyles"
        >
          <div :class="$style.inner">
            <div v-if="$slots.icon" :class="$style.iconWrapper">
              <slot name="icon" />
            </div>

            <div :class="$style.content">
              <slot />
            </div>

            <footer :class="$style.footer">
              <slot name="actions">
                <button @click="handleClose" :class="$style.closeButton">
                  {{ buttonText }}
                </button>
              </slot>
            </footer>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: boolean;
  buttonText?: string;
  color?: string;       /* Color del texto de acción */
  // Personalización Neumórfica
  baseColor?: string;   /* El color de fondo (debe coincidir con el del sitio) */
  distance?: number;    /* Profundidad del relieve (4-20) */
  blur?: number;        /* Suavidad de la sombra */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'ENTENDIDO',
  color: '#4f46e5',
  baseColor: '#e0e5ec',
  distance: 12,
  blur: 24,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const neuStyles = computed(() => {
  const d = props.distance;
  const b = props.blur;
  // Cálculo de sombras: Oscura (shadow) y Clara (highlight)
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.color,
    '--n-dist': `${d}px`,
    '--n-blur': `${b}px`,
    '--n-shadow-dark': 'rgba(163, 177, 198, 0.7)',
    '--n-shadow-light': 'rgba(255, 255, 255, 0.9)',
  };
});
</script>

<style module>
/* --- Animación --- */
.neuFadeEnterActive { animation: neuIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
.neuFadeLeaveActive { transition: all 0.3s ease-in; opacity: 0; transform: scale(0.9); }

@keyframes neuIn {
  from { transform: scale(0.8); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

/* --- Estructura --- */
.overlay {
  position: fixed; inset: 0;
  background: rgba(224, 229, 236, 0.5); /* Opacidad baja para dejar ver el fondo */
  backdrop-filter: blur(8px);
  display: flex; align-items: center; justify-content: center;
  z-index: 9999; padding: 20px;
}

.neuModal {
  background: var(--n-bg);
  width: 100%;
  padding: 3rem 2.5rem;
  border-radius: 40px;
  position: relative;
  /* El secreto del relieve: sombras proyectadas opuestas */
  box-shadow: 
    var(--n-dist) var(--n-dist) var(--n-blur) var(--n-shadow-dark),
    calc(var(--n-dist) * -1) calc(var(--n-dist) * -1) var(--n-blur) var(--n-shadow-light);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.sm { max-width: 340px; }
.md { max-width: 440px; }
.lg { max-width: 600px; }

.inner { display: flex; flex-direction: column; gap: 2rem; align-items: center; }

/* --- Icono Hundido --- */
.iconWrapper {
  width: 80px; height: 80px; border-radius: 50%;
  background: var(--n-bg);
  display: flex; align-items: center; justify-content: center;
  box-shadow: 
    inset 6px 6px 12px var(--n-shadow-dark),
    inset -6px -6px 12px var(--n-shadow-light);
}

.content {
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  color: #6d7c90;
  line-height: 1.6;
  text-align: center;
}

/* --- Botón Neumórfico --- */
.closeButton {
  background: var(--n-bg);
  color: var(--n-accent);
  border: none;
  padding: 1rem 3rem;
  font-size: 1rem;
  font-weight: 800;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 
    8px 8px 16px var(--n-shadow-dark),
    -8px -8px 16px var(--n-shadow-light);
}

.closeButton:hover {
  transform: translateY(-2px);
  filter: brightness(1.02);
}

.closeButton:active {
  transform: translateY(0);
  /* Efecto hundido al presionar */
  box-shadow: 
    inset 6px 6px 12px var(--n-shadow-dark),
    inset -6px -6px 12px var(--n-shadow-light);
}
</style>