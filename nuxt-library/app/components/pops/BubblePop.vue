<template>
  <Teleport to="body">
    <Transition :name="$style.bubblePop">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.bubbleModal, $style[size]]" 
          :style="bubbleStyles"
        >
          <div v-if="glossy" :class="$style.gloss" />
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <footer :class="$style.footer">
              <slot name="actions">
                <button @click="handleClose" :class="$style.closeAction">
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
  color?: string;
  // Configuración Bubble
  irregularity?: number;   /* Qué tan "deforme" es (1-10) */
  glossy?: boolean;        /* ¿Efecto de brillo de cristal? */
  bounce?: boolean;        /* ¿Animación con rebote elástico? */
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '¡Entendido!',
  color: '#10b981',
  irregularity: 5,
  glossy: true,
  bounce: true,
  size: 'md'
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const bubbleStyles = computed(() => {
  const irr = props.irregularity;
  // Generamos un border-radius orgánico basado en la irregularidad
  const br = `${50 + irr}% ${50 - irr}% ${45 + irr}% ${55 - irr}% / ${45 - irr}% ${55 + irr}% ${50 - irr}% ${50 + irr}%`;
  
  return {
    '--b-accent': props.color,
    '--b-radius': br,
    '--b-anim': props.bounce ? 'bubbleBounce 0.6s cubic-bezier(0.34, 1.56, 0.64, 1)' : 'simpleFade 0.3s ease',
  };
});
</script>

<style module>
/* --- Animaciones --- */
.bubblePopEnterActive { animation: var(--b-anim); }
.bubblePopLeaveActive { animation: var(--b-anim) reverse; }

@keyframes bubbleBounce {
  0% { transform: scale(0.5) translateY(40px); opacity: 0; }
  60% { transform: scale(1.05) translateY(-5px); }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}

@keyframes simpleFade {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* --- Estructura --- */
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(16, 185, 129, 0.1);
  backdrop-filter: blur(10px); 
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
}

.bubbleModal {
  background: white;
  padding: 3rem 2.5rem;
  width: 90%;
  border-radius: var(--b-radius);
  border: 4px solid var(--b-accent);
  box-shadow: 0 20px 60px -15px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
  text-align: center;
}

.sm { max-width: 340px; }
.md { max-width: 420px; }
.lg { max-width: 550px; }

/* --- Brillo de Cristal --- */
.gloss {
  position: absolute;
  top: 10%;
  left: 15%;
  width: 30%;
  height: 15%;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 50%;
  filter: blur(8px);
  pointer-events: none;
}

.inner { display: flex; flex-direction: column; gap: 2rem; align-items: center; }

.content {
  font-family: 'Rounded Mplus 1c', 'Quicksand', sans-serif;
  font-weight: 600;
  color: #1f2937;
  line-height: 1.5;
  font-size: 1.1rem;
}

/* --- Acción Principal --- */
.closeAction {
  background: var(--b-accent);
  color: white;
  border: none;
  padding: 0.8rem 2.2rem;
  border-radius: 50px;
  font-weight: 800;
  font-size: 1rem;
  cursor: pointer;
  box-shadow: 0 4px 0px rgba(0, 0, 0, 0.1);
  transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.closeAction:hover {
  transform: scale(1.1) rotate(2deg);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.closeAction:active {
  transform: scale(0.9);
}
</style>