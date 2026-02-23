<template>
  <Teleport to="body">
    <Transition :name="$style.glassFade">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.glassModal, $style[size]]" 
          :style="glassStyles"
        >
          <div :class="$style.glassReflection" />
          
          <div :class="$style.inner">
            <div :class="$style.content">
              <slot />
            </div>
            
            <footer v-if="$slots.footer || showClose" :class="$style.footer">
              <slot name="footer">
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
  // Personalización Glass
  blur?: string;          /* Intensidad del desenfoque (ej: 20px) */
  opacity?: number;       /* Transparencia (0 a 1) */
  saturation?: string;    /* Qué tanto resalta los colores de fondo (ej: 180%) */
  borderIntensity?: number; /* Brillo del borde blanco (0 a 1) */
  size?: 'sm' | 'md' | 'lg';
  showClose?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'Continuar',
  blur: '25px',
  opacity: 0.65,
  saturation: '160%',
  borderIntensity: 0.4,
  size: 'md',
  showClose: true
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const glassStyles = computed(() => ({
  '--g-blur': props.blur,
  '--g-opacity': props.opacity,
  '--g-saturation': props.saturation,
  '--g-border': `rgba(255, 255, 255, ${props.borderIntensity})`,
}));
</script>

<style module>
/* --- Animación de Entrada --- */
.glassFadeEnterActive {
  transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
.glassFadeLeaveActive {
  transition: all 0.3s ease-in;
}

.glassFadeEnterFrom,
.glassFadeLeaveTo {
  opacity: 0;
  transform: scale(0.92) translateY(10px);
}

/* --- Overlay --- */
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(0, 0, 0, 0.15); /* Oscurecimiento mínimo para notar el blur */
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
  padding: 20px;
}

/* --- El Cristal --- */
.glassModal {
  background: rgba(255, 255, 255, var(--g-opacity));
  backdrop-filter: blur(var(--g-blur)) saturate(var(--g-saturation));
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(var(--g-saturation));
  
  border-radius: 28px;
  border: 1px solid var(--g-border);
  box-shadow: 
    0 20px 40px rgba(0, 0, 0, 0.1),
    inset 0 0 0 1px rgba(255, 255, 255, 0.1); /* Brillo interno extra */
  
  width: 100%;
  position: relative;
  overflow: hidden;
}

.sm { max-width: 380px; }
.md { max-width: 520px; }
.lg { max-width: 720px; }

/* --- Reflejo de Cristal --- */
.glassReflection {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(
    45deg,
    transparent 45%,
    rgba(255, 255, 255, 0.1) 48%,
    rgba(255, 255, 255, 0.3) 50%,
    rgba(255, 255, 255, 0.1) 52%,
    transparent 55%
  );
  transform: rotate(-20deg);
  pointer-events: none;
}

.inner { 
  padding: 2.5rem;
  display: flex; 
  flex-direction: column; 
  gap: 1.5rem;
  position: relative;
  z-index: 2;
}

.content {
  font-family: 'Inter', system-ui, sans-serif;
  color: #1a1a1a;
  line-height: 1.6;
}

.footer {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}

/* --- Botón Glass --- */
.closeAction {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  color: #000;
  border: 1px solid rgba(255, 255, 255, 0.5);
  padding: 0.8rem 2.5rem;
  border-radius: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.closeAction:hover {
  background: #ffffff;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}
</style>