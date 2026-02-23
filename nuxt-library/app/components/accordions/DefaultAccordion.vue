<template>
  <div 
    :class="[
      $style.accordion, 
      $style[variant]
    ]" 
    :style="accordionStyles"
  >
    <button 
      @click="isOpen = !isOpen" 
      :aria-expanded="isOpen"
      :class="$style.trigger"
    >
      <div :class="$style.titleWrapper">
        <slot name="icon" v-if="$slots.icon" />
        <span :class="$style.title">{{ title }}</span>
      </div>
      
      <div :class="[$style.arrow, { [$style.rotated]: isOpen }]">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="6 9 12 15 18 9"></polyline>
        </svg>
      </div>
    </button>

    <transition
      @enter="startTransition"
      @after-enter="endTransition"
      @before-leave="startTransition"
      @after-leave="endTransition"
    >
      <div v-show="isOpen" :class="$style.contentWrapper">
        <div :class="$style.content">
          <slot />
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

interface Props {
  title: string;
  variant?: 'modern' | 'cyber' | 'brutal' | 'glass' | 'neu';
  accentColor?: string;
  initialOpen?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'modern',
  accentColor: '#3b82f6',
  initialOpen: false
});

const isOpen = ref(props.initialOpen);

const accordionStyles = computed(() => ({
  '--acc-accent': props.accentColor,
  '--acc-bg': props.variant === 'cyber' ? '#0a0a0b' : 'transparent',
}));

// Funciones para animar la altura dinámicamente sin hardcode
const startTransition = (el: Element) => {
  (el as HTMLElement).style.height = (el as HTMLElement).scrollHeight + 'px';
};
const endTransition = (el: Element) => {
  (el as HTMLElement).style.height = '';
};
</script>

<style module>
.accordion {
  width: 100%;
  overflow: hidden;
  transition: all 0.3s ease;
}

.trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: none;
  border: none;
  cursor: pointer;
  color: inherit;
  font-family: inherit;
  text-align: left;
}

.titleWrapper {
  display: flex;
  align-items: center;
  gap: 12px;
}

.title {
  font-weight: 700;
  font-size: 1rem;
}

.arrow {
  transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  opacity: 0.6;
}

.rotated {
  transform: rotate(180deg);
  opacity: 1;
}

.contentWrapper {
  overflow: hidden;
  transition: height 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}

.content {
  padding: 0 20px 20px 20px;
  font-size: 0.95rem;
  line-height: 1.5;
  opacity: 0.8;
}

/* --- VARIANTES DE ESTILO --- */

/* MODERN (Minimal) */
.modern {
  border-bottom: 1px solid rgba(0,0,0,0.06);
}

/* CYBER */
.cyber {
  border: 1px solid color-mix(in srgb, var(--acc-accent), transparent 80%);
  background: #0a0a0b;
  margin-bottom: 8px;
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%);
}
.cyber .title { text-transform: uppercase; color: var(--acc-accent); letter-spacing: 1px; }

/* BRUTAL */
.brutal {
  border: 3px solid #000;
  background: #fff;
  margin-bottom: 12px;
  box-shadow: 4px 4px 0 #000;
}
.brutal:active { transform: translate(2px, 2px); box-shadow: 0px 0px 0 #000; }
.brutal .title { text-transform: uppercase; font-weight: 900; }

/* GLASS */
.glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  margin-bottom: 8px;
  color: white;
}

/* NEU */
.neu {
  background: #e0e5ec;
  border-radius: 20px;
  box-shadow: 6px 6px 12px #a3b1c6, -6px -6px 12px #ffffff;
  margin-bottom: 16px;
}
</style>