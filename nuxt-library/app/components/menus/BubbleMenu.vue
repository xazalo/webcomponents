<template>
  <nav 
    v-bind="$attrs" 
    :class="[$style.bubbleMenu, $style[size]]" 
    :style="bubbleStyles"
  >
    <div :class="[$style.cell, $style.headerCell]">
      <div v-if="glossy" :class="$style.glare" />
      <div :class="$style.headerContent">
        <span :class="$style.label">{{ prefix }}</span>
        <h2 :class="$style.title">{{ activeLabel }}</h2>
      </div>
    </div>

    <div :class="$style.list">
      <a
        v-for="(link, index) in links"
        :key="link.path"
        :href="link.path"
        :class="[
          $style.cell,
          $style.linkCell,
          { [$style.active]: modelValue === link.path },
        ]"
        :style="{ '--accent-color': palette[index % palette.length] }"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div v-if="glossy" :class="$style.miniGlare" />
        
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span v-if="link.description" :class="$style.linkDesc">{{ link.description }}</span>
        </div>

        <div :class="$style.indicator">
          <slot name="indicator">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4" stroke-linecap="round" stroke-linejoin="round">
              <path d="M9 18l6-6-6-6" />
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
import { computed } from "vue";
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
  // Estilo
  palette?: string[];      /* Array de colores para las burbujas */
  borderRadius?: string;   /* Nivel de redondeo */
  glossy?: boolean;        /* ¿Mostrar reflejos de luz? */
  size?: 'sm' | 'md' | 'lg';
  elasticity?: number;     /* Cuánto se escala al hacer hover (1.0 a 1.2) */
}

const props = withDefaults(defineProps<Props>(), {
  buttonText: '',
  prefix: 'MENU',
  palette: () => ["#ff8dfb", "#70e0ff", "#8dffbe", "#ffdb70"],
  borderRadius: '30px',
  glossy: true,
  size: 'md',
  elasticity: 1.05
});

const emit = defineEmits(["update:modelValue", "action"]);

const activeLabel = computed(() => {
  return props.links.find((l) => l.path === props.modelValue)?.label || "MAIN";
});

const bubbleStyles = computed(() => ({
  '--b-radius': props.borderRadius,
  '--b-elastic': props.elasticity,
  '--b-bg-main': '#f0f4f8'
}));
</script>

<style module>
.bubbleMenu {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 15px;
  background: var(--b-bg-main);
  border-radius: calc(var(--b-radius) + 10px);
  width: 100%;
  max-width: 400px;
  font-family: 'Rounded Mplus 1c', 'Inter', sans-serif;
}

.cell {
  background: white;
  border-radius: var(--b-radius);
  padding: 16px 22px;
  position: relative;
  overflow: hidden;
  border: none;
  box-shadow: 
    0 8px 20px rgba(0,0,0,0.04),
    inset 2px 4px 8px rgba(255, 255, 255, 0.8),
    inset -2px -4px 8px rgba(0, 0, 0, 0.02);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* --- Header --- */
.headerCell {
  background: linear-gradient(135deg, #ffffff 0%, #f9fafb 100%);
}

.label {
  font-size: 0.65rem;
  font-weight: 800;
  color: #adb5bd;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.title {
  margin: 4px 0 0;
  font-size: 1.5rem;
  font-weight: 900;
  color: #2d3436;
  line-height: 1;
}

/* --- List Items --- */
.list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.linkCell {
  display: flex;
  align-items: center;
  justify-content: space-between;
  text-decoration: none;
  cursor: pointer;
}

.linkCell:hover {
  transform: scale(var(--b-elastic)) translateY(-2px);
  background: var(--accent-color);
  box-shadow: 0 15px 30px rgba(0,0,0,0.1);
}

.active {
  background: var(--accent-color) !important;
  transform: scale(0.96);
  box-shadow: inset 4px 4px 12px rgba(0, 0, 0, 0.12);
}

.active .linkTitle, .active .linkDesc, .active .indicator,
.linkCell:hover .linkTitle, .linkCell:hover .linkDesc, .linkCell:hover .indicator {
  color: #fff !important;
}

/* --- Content --- */
.linkTitle {
  display: block;
  font-weight: 800;
  font-size: 1.1rem;
  color: #495057;
  transition: color 0.3s ease;
}

.linkDesc {
  font-size: 0.75rem;
  font-weight: 600;
  color: #adb5bd;
  transition: color 0.3s ease;
}

.indicator {
  color: #dee2e6;
  display: flex;
  transition: transform 0.3s ease;
}

.linkCell:hover .indicator {
  transform: translateX(5px) scale(1.1);
}

/* --- Reflejos (Glossy) --- */
.glare {
  position: absolute;
  top: 6px;
  left: 10%;
  width: 80%;
  height: 6px;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 20px;
  filter: blur(1px);
}

.miniGlare {
  position: absolute;
  top: 8px;
  left: 20px;
  width: 25px;
  height: 5px;
  background: rgba(255, 255, 255, 0.4);
  border-radius: 10px;
  pointer-events: none;
}

/* --- Action Button --- */
.actionBtn {
  margin-top: 8px;
  background: #2d3436;
  color: white;
  font-weight: 800;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  cursor: pointer;
  text-align: center;
}

.actionBtn:hover {
  filter: brightness(1.2);
  transform: translateY(-2px);
}

.actionBtn:active {
  transform: scale(0.95);
}

/* --- Tamaños --- */
.sm .cell { padding: 12px 18px; }
.sm .title { font-size: 1.2rem; }
.lg .cell { padding: 20px 28px; }
.lg .title { font-size: 1.8rem; }
</style>