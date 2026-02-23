<template>
  <Teleport to="body">
    <Transition :name="$style.bentoFade">
      <div 
        v-if="modelValue" 
        :class="$style.overlay" 
        @click.self="handleClose"
      >
        <div 
          v-bind="$attrs" 
          :class="[$style.bentoModal, $style[size]]" 
          :style="bentoStyles"
        >
          <div v-if="showHeaderDecor" :class="$style.bentoHeader">
            <div :class="$style.dot"></div>
            <div :class="$style.line"></div>
            <div :class="$style.lineShort"></div>
          </div>
          
          <div :class="$style.inner">
            <header v-if="$slots.header" :class="$style.slotHeader">
              <slot name="header" />
            </header>

            <div :class="$style.content">
              <slot />
            </div>
            
            <footer :class="$style.footer">
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
  color?: string;
  // Configuración Bento
  radius?: string;        /* Radio de las esquinas (ej: 2rem) */
  blur?: string;          /* Intensidad del desenfoque del overlay */
  size?: 'sm' | 'md' | 'lg';
  showHeaderDecor?: boolean;
  closeOnEsc?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: 'Entendido',
  color: '#4f46e5',
  radius: '1.5rem',
  blur: '12px',
  size: 'md',
  showHeaderDecor: true,
  closeOnEsc: true
});

const emit = defineEmits(['update:modelValue', 'close']);

const handleClose = () => {
  emit('update:modelValue', false);
  emit('close');
};

const bentoStyles = computed(() => ({
  '--b-accent': props.color,
  '--b-radius': props.radius,
  '--b-blur': props.blur,
}));
</script>

<style module>
/* --- Animación --- */
.bentoFadeEnterActive,
.bentoFadeLeaveActive {
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.bentoFadeEnterFrom,
.bentoFadeLeaveTo {
  opacity: 0;
  transform: scale(0.95) translateY(10px);
}

/* --- Overlay --- */
.overlay {
  position: fixed; 
  inset: 0; 
  background: rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(var(--b-blur)); 
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 9999;
  padding: 20px;
}

/* --- Modal --- */
.bentoModal {
  background: #ffffff;
  padding: 1.75rem;
  width: 100%;
  border-radius: var(--b-radius);
  border: 1px solid rgba(0, 0, 0, 0.08);
  box-shadow: 
    0 30px 60px -12px rgba(0, 0, 0, 0.15),
    0 18px 36px -18px rgba(0, 0, 0, 0.1),
    0 0 0 1px rgba(0, 0, 0, 0.04);
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease;
}

/* --- Tamaños --- */
.sm { max-width: 360px; }
.md { max-width: 480px; }
.lg { max-width: 640px; }

/* --- Decoración Bento --- */
.bentoHeader {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 1.5rem;
  opacity: 0.4;
}

.dot { width: 10px; height: 10px; border-radius: 4px; background: var(--b-accent); }
.line { width: 40px; height: 4px; border-radius: 10px; background: #e5e7eb; }
.lineShort { width: 15px; height: 4px; border-radius: 10px; background: #f3f4f6; }

.inner { display: flex; flex-direction: column; gap: 1.5rem; }

.slotHeader {
  font-family: system-ui, sans-serif;
  font-weight: 700;
  font-size: 1.25rem;
  color: #111827;
}

.content {
  font-family: system-ui, -apple-system, sans-serif;
  color: #4b5563;
  line-height: 1.6;
  font-size: 0.95rem;
}

.footer {
  margin-top: 0.5rem;
}

/* --- Acción Principal --- */
.closeAction {
  background: #f9fafb;
  color: #111827;
  border: 1px solid #e5e7eb;
  padding: 0.9rem 1.5rem;
  border-radius: calc(var(--b-radius) * 0.5); /* Radio proporcional */
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.closeAction:hover {
  background: #ffffff;
  border-color: var(--b-accent);
  color: var(--b-accent);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transform: translateY(-2px);
}

.closeAction:active {
  transform: translateY(0);
}
</style>