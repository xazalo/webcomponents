<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.glassMenu, $style[size]]" 
    :style="glassStyles"
  >
    <div :class="[$style.cell, $style.headerCell]">
      <div v-if="shine" :class="$style.shineEffect" />
      <span :class="$style.label">{{ prefix }}</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
    </div>

    <div :class="$style.list">
      <a 
        v-for="link in links" 
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell, 
          $style.linkCell, 
          { [$style.active]: modelValue === link.path }
        ]"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        
        <div :class="$style.glassArrow">
          <slot name="icon-arrow">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
          </slot>
        </div>
      </a>
    </div>

    <button 
      v-if="buttonText"
      :class="[$style.cell, $style.actionBtn]" 
      @click="$emit('action')"
    >
      {{ buttonText }}
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

interface Props {
  modelValue: string;
  links: NavLink[];
  buttonText?: string;
  prefix?: string;
  // Configuración Glass
  tintColor?: string;     /* RGB (ej: "255, 255, 255") */
  blurIntensity?: number; /* En píxeles */
  saturation?: number;    /* En porcentaje (ej: 180) */
  borderRadius?: string;
  size?: 'sm' | 'md' | 'lg';
  shine?: boolean;        /* Destello animado en el header */
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '',
  prefix: '_NAV',
  tintColor: '255, 255, 255',
  blurIntensity: 12,
  saturation: 180,
  borderRadius: '20px',
  size: 'md',
  shine: true
});

const emit = defineEmits(['update:modelValue', 'action']);

const activeLabel = computed(() => {
  return props.links.find(l => l.path === props.modelValue)?.label || 'MENU';
});

const glassStyles = computed(() => ({
  '--g-tint': props.tintColor,
  '--g-blur': `${props.blurIntensity}px`,
  '--g-sat': `${props.saturation}%`,
  '--g-radius': props.borderRadius,
  '--g-border': `rgba(${props.tintColor}, 0.4)`,
  '--g-bg': `rgba(${props.tintColor}, 0.1)`,
  '--g-active': `rgba(${props.tintColor}, 0.25)`,
}));
</script>

<style module>
.glassMenu {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 20px;
  background: rgba(0, 0, 0, 0.15); /* Contenedor oscuro para resaltar el cristal */
  backdrop-filter: blur(calc(var(--g-blur) * 0.8));
  border-radius: calc(var(--g-radius) + 10px);
  width: 100%;
  max-width: 380px;
  border: 1px solid var(--g-border);
  font-family: 'Inter', system-ui, sans-serif;
}

.cell {
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur)) saturate(var(--g-sat));
  -webkit-backdrop-filter: blur(var(--g-blur)) saturate(var(--g-sat));
  border-radius: var(--g-radius);
  padding: 16px 20px;
  border: 1px solid var(--g-border);
  position: relative;
  overflow: hidden;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
}

/* --- Header --- */
.headerCell {
  background: rgba(var(--g-tint), 0.05);
  border-bottom: 2px solid var(--g-border);
}

.shineEffect {
  position: absolute;
  top: -100%;
  left: -100%;
  width: 300%;
  height: 300%;
  background: linear-gradient(
    45deg, 
    transparent 0%, 
    rgba(255, 255, 255, 0.05) 45%, 
    rgba(255, 255, 255, 0.2) 50%, 
    rgba(255, 255, 255, 0.05) 55%, 
    transparent 100%
  );
  transform: rotate(25deg);
  animation: shine-loop 6s infinite;
  pointer-events: none;
}

@keyframes shine-loop {
  0% { transform: translate(-30%, -30%) rotate(25deg); }
  100% { transform: translate(30%, 30%) rotate(25deg); }
}

.label {
  font-size: 0.65rem;
  font-weight: 800;
  color: rgba(255, 255, 255, 0.5);
  letter-spacing: 2px;
}

.title {
  margin: 4px 0 0;
  font-size: 1.6rem;
  font-weight: 300;
  color: #fff;
  letter-spacing: -0.5px;
}

/* --- List --- */
.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
}

.linkCell:hover {
  background: var(--g-active);
  transform: translateX(8px);
  border-color: rgba(255, 255, 255, 0.8);
}

.active {
  background: rgba(255, 255, 255, 0.3) !important;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.9);
}

.linkTitle {
  display: block;
  font-weight: 600;
  font-size: 1.1rem;
  color: #fff;
}

.linkDesc {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.6);
}

.glassArrow {
  color: #fff;
  opacity: 0.4;
  transition: all 0.3s ease;
}

.linkCell:hover .glassArrow {
  opacity: 1;
  transform: translateX(3px);
}

/* --- Action Button --- */
.actionBtn {
  margin-top: 8px;
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  font-weight: 700;
  cursor: pointer;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.actionBtn:hover {
  background: rgba(255, 255, 255, 0.2);
}

/* --- Tamaños --- */
.sm { max-width: 320px; padding: 15px; }
.sm .title { font-size: 1.3rem; }
.lg { max-width: 440px; }
.lg .cell { padding: 20px 26px; }
</style>