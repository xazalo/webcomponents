<template>
  <Teleport to="body">
    <Transition :name="$style.clayPop">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.clayModal, $style[size]]" 
          :style="clayStyles"
        >
          <div v-if="highLight" :class="$style.decorLight"></div>
          
          <div :class="$style.inner">
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
  color?: string;       /* Color del botón de acción */
  baseColor?: string;   /* Color del cuerpo del modal */
  // Configuración Clay
  radius?: string;      /* Redondez (ej: 40px) */
  depth?: number;       /* Intensidad de sombras internas (0-10) */
  highLight?: boolean;  /* ¿Brillo superior decorativo? */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '¡HECHO!',
  color: '#ff7eb9',
  baseColor: '#ffffff',
  radius: '45px',
  depth: 5,
  highLight: true,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const clayStyles = computed(() => ({
  '--c-accent': props.color,
  '--c-bg': props.baseColor,
  '--c-radius': props.radius,
  '--c-depth-shadow': `inset 0 -${props.depth * 2}px ${props.depth * 4}px rgba(0, 0, 0, 0.08)`,
  '--c-depth-light': `inset 0 ${props.depth * 2}px ${props.depth * 4}px rgba(255, 255, 255, 1)`,
}));
</script>

<style module>
/* --- Animación --- */
.clayPopEnterActive {
  animation: clayBounce 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.clayPopLeaveActive {
  animation: clayBounce 0.3s reverse ease-in;
}

@keyframes clayBounce {
  0% { transform: scale(0.4) translateY(50px); opacity: 0; }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}

/* --- Overlay --- */
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(226, 232, 240, 0.7); 
  backdrop-filter: blur(12px);
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
}

/* --- El Modal de Arcilla --- */
.clayModal {
  background: var(--c-bg);
  width: 90%;
  padding: 3rem;
  position: relative;
  border-radius: var(--c-radius);
  
  /* Sombra externa + doble sombra interna (Clay Effect) */
  box-shadow: 
    0 25px 50px -12px rgba(0, 0, 0, 0.15),
    var(--c-depth-light),
    var(--c-depth-shadow);
  
  border: 1px solid rgba(255, 255, 255, 0.4);
}

.sm { max-width: 320px; }
.md { max-width: 420px; }
.lg { max-width: 600px; }

/* --- Brillo superior --- */
.decorLight {
  position: absolute;
  top: 8%;
  left: 10%;
  width: 25%;
  height: 15%;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 50%;
  filter: blur(12px);
  transform: rotate(-15deg);
  pointer-events: none;
}

.inner { display: flex; flex-direction: column; gap: 2rem; align-items: center; }

.content {
  font-family: 'Quicksand', system-ui, sans-serif;
  font-size: 1.15rem;
  font-weight: 700;
  color: #475569;
  line-height: 1.5;
  text-align: center;
}

/* --- Botón Clay --- */
.closeButton {
  background: var(--c-accent);
  color: white;
  border: none;
  padding: 1rem 2.5rem;
  font-size: 1.1rem;
  font-weight: 800;
  border-radius: calc(var(--c-radius) * 0.5);
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  
  box-shadow: 
    0 12px 24px -6px rgba(0, 0, 0, 0.15),
    inset 0 4px 10px rgba(255, 255, 255, 0.5),
    inset 0 -4px 10px rgba(0, 0, 0, 0.2);
}

.closeButton:hover {
  transform: translateY(-4px) scale(1.03);
  box-shadow: 
    0 15px 30px -8px rgba(0, 0, 0, 0.2),
    inset 0 4px 10px rgba(255, 255, 255, 0.6),
    inset 0 -4px 10px rgba(0, 0, 0, 0.25);
}

.closeButton:active {
  transform: scale(0.95);
  box-shadow: 
    0 5px 10px rgba(0, 0, 0, 0.1),
    inset 0 2px 5px rgba(255, 255, 255, 0.4),
    inset 0 -2px 5px rgba(0, 0, 0, 0.2);
}
</style>