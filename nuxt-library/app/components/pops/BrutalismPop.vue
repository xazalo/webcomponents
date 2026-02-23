<template>
  <Teleport to="body">
    <Transition :name="$style.brutalPop">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.brutalModal, $style[size]]" 
          :style="brutalStyles"
        >
          <div :class="$style.brutalHeader">
            <span :class="$style.title">{{ headerText }}</span>
            <div :class="$style.headerControls">
              <button :class="$style.controlBtn">_</button>
              <button @click="handleClose" :class="$style.miniClose">×</button>
            </div>
          </div>
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <footer :class="$style.footer">
              <slot name="actions">
                <button @click="handleClose" :class="$style.mainAction">
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
  headerText?: string;
  color?: string;
  // Configuración Brutalista
  borderWidth?: string;   /* Grosor de los trazos (ej: 4px) */
  shadowOffset?: string;  /* Desplazamiento sombra (ej: 12px) */
  tilt?: number;          /* Inclinación leve (ej: -1) */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'OK_CONFIRM',
  headerText: 'CRITICAL_MESSAGE.SYS',
  color: '#ff3e00',
  borderWidth: '4px',
  shadowOffset: '12px',
  tilt: 0,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const brutalStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-border': props.borderWidth,
  '--b-shadow': props.shadowOffset,
  '--b-tilt': `${props.tilt}deg`,
}));
</script>

<style module>
/* --- Animación Pop --- */
.brutalPopEnterActive {
  animation: brutalSlide 0.25s cubic-bezier(0.18, 0.89, 0.32, 1.28);
}
.brutalPopLeaveActive {
  animation: brutalSlide 0.15s reverse ease-in;
}

@keyframes brutalSlide {
  from { transform: scale(0.5) rotate(5deg); opacity: 0; }
  to { transform: scale(1) rotate(var(--b-tilt)); opacity: 1; }
}

/* --- Estructura --- */
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(0, 0, 0, 0.4); /* Oscurecemos para que el blanco resalte */
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
  padding: 20px;
}

.brutalModal {
  background: #ffffff;
  width: 100%;
  border: var(--b-border) solid #000000;
  border-radius: 0px; 
  box-shadow: var(--b-shadow) var(--b-shadow) 0px 0px #000000;
  position: relative;
  overflow: hidden;
  transform: rotate(var(--b-tilt));
}

.sm { max-width: 320px; }
.md { max-width: 420px; }
.lg { max-width: 580px; }

/* --- Cabecera --- */
.brutalHeader {
  background: #000000;
  padding: 0.6rem 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #ffffff;
}

.title {
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.8rem;
  font-weight: 900;
  text-transform: uppercase;
}

.headerControls { display: flex; gap: 8px; }

.controlBtn, .miniClose {
  background: #ffffff;
  border: 2px solid #000000;
  color: #000;
  font-weight: 900;
  cursor: pointer;
  padding: 0px 8px;
  font-size: 1rem;
}

.miniClose:hover {
  background: var(--b-accent);
  color: white;
}

/* --- Cuerpo --- */
.inner { 
  padding: 2.5rem 2rem;
  display: flex; 
  flex-direction: column; 
  gap: 2rem; 
}

.content {
  font-family: 'Courier New', Courier, monospace;
  font-weight: 800;
  color: #000000;
  line-height: 1.3;
  font-size: 1.2rem;
}

/* --- Botón Brutal --- */
.mainAction {
  background: var(--b-accent);
  color: #ffffff;
  border: var(--b-border) solid #000000;
  padding: 1.2rem;
  font-family: 'Courier New', Courier, monospace;
  font-weight: 900;
  font-size: 1.2rem;
  text-transform: uppercase;
  cursor: pointer;
  width: 100%;
  box-shadow: 6px 6px 0px 0px #000000;
  transition: transform 0.1s, box-shadow 0.1s;
}

.mainAction:hover {
  transform: translate(-3px, -3px);
  box-shadow: 9px 9px 0px 0px #000000;
}

.mainAction:active {
  transform: translate(6px, 6px);
  box-shadow: 0px 0px 0px 0px #000000;
}

/* Responsive */
@media (max-width: 480px) {
  .brutalModal { box-shadow: 8px 8px 0px 0px #000000; }
  .inner { padding: 1.5rem; }
}
</style>