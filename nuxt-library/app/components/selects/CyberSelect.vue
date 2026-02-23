<template>
  <div v-bind="$attrs" :class="$style.cyberRoot" :style="cyberStyles">
    <label v-if="label" :class="$style.label">
      <span :class="$style.labelPrefix">>></span> {{ label }}
    </label>
    
    <div :class="$style.container">
      <button 
        type="button"
        :class="[$style.trigger, { [$style.isOpen]: isOpen }]" 
        @click="isOpen = !isOpen"
      >
        <div :class="$style.scanline" />
        <span :class="$style.text">{{ selectedLabel || placeholder }}</span>
        <div :class="$style.iconWrapper">
          <svg :class="[$style.chevron, { [$style.rotate]: isOpen }]" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="4">
            <path d="m6 9 12 0-6 6-6-6Z" fill="currentColor"/>
          </svg>
        </div>
      </button>

      <Transition :name="$style.cyberGlitch">
        <ul v-if="isOpen" :class="$style.options">
          <li 
            v-for="opt in options" 
            :key="opt.value" 
            :class="[$style.option, { [$style.active]: modelValue === opt.value }]"
            @click="selectOption(opt.value)"
          >
            <div :class="$style.optionHoverDecor" />
            <span :class="$style.optionText">{{ opt.label }}</span>
            <span v-if="modelValue === opt.value" :class="$style.selectedTag">SEL_01</span>
          </li>
        </ul>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface Props {
  modelValue: any;
  options: { label: string; value: any }[];
  label?: string;
  placeholder?: string;
  color?: string;       /* Color del Neón */
  cutSize?: number;     /* Tamaño del recorte de esquina (px) */
  glitch?: boolean;     /* ¿Animación de interferencia? */
  glow?: boolean;       /* ¿Brillo exterior neón? */
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: 'SELECT_DATA',
  color: '#00f3ff',
  cutSize: 12,
  glitch: true,
  glow: true
});

const emit = defineEmits(['update:modelValue']);
const isOpen = ref(false);

const selectedLabel = computed(() => 
  props.options.find(o => o.value === props.modelValue)?.label
);

const cyberStyles = computed(() => ({
  '--c-neon': props.color,
  '--c-cut': `${props.cutSize}px`,
  '--c-glow': props.glow ? `0 0 15px color-mix(in srgb, ${props.color} 40%, transparent)` : 'none',
  '--c-glitch-anim': props.glitch ? 'cyberGlitch 0.2s steps(2) infinite' : 'none'
}));

const selectOption = (val: any) => {
  emit('update:modelValue', val);
  isOpen.value = false;
};
</script>

<style module>
/* --- Animación Glitch --- */
.cyberGlitchEnterActive { animation: cyberGlitch 0.3s steps(4); }
.cyberGlitchLeaveActive { opacity: 0; transition: opacity 0.2s; }

@keyframes cyberGlitch {
  0% { transform: translate(5px, 0); clip-path: inset(10% 0 30% 0); }
  50% { transform: translate(-5px, 0); clip-path: inset(40% 0 10% 0); }
  100% { transform: translate(0); clip-path: inset(0); }
}

.cyberRoot { display: flex; flex-direction: column; gap: 0.6rem; width: 100%; font-family: 'JetBrains Mono', monospace; }

.label { 
  font-size: 0.75rem; font-weight: 800; color: var(--c-neon); 
  letter-spacing: 2px; text-transform: uppercase;
}
.labelPrefix { opacity: 0.5; margin-right: 4px; }

.container { position: relative; }

/* --- Trigger --- */
.trigger {
  width: 100%;
  padding: 0.8rem 1.2rem;
  background: #000;
  color: #fff;
  border: 1px solid rgba(255,255,255,0.1);
  border-left: 3px solid var(--c-neon);
  display: flex; justify-content: space-between; align-items: center;
  cursor: pointer; position: relative; overflow: hidden;
  clip-path: polygon(
    var(--c-cut) 0%, 100% 0%, 
    100% calc(100% - var(--c-cut)), 
    calc(100% - var(--c-cut)) 100%, 0% 100%, 0% var(--c-cut)
  );
  box-shadow: var(--c-glow);
}

.scanline {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(255, 255, 255, 0.05) 50%);
  background-size: 100% 4px;
  pointer-events: none;
}

.text { font-weight: 700; text-transform: uppercase; letter-spacing: 1px; z-index: 2; }

/* --- Menu --- */
.options {
  position: absolute; top: 120%; left: 0; width: 100%;
  background: #0a0a0a;
  border: 1px solid var(--c-neon);
  list-style: none; padding: 4px; margin: 0; z-index: 100;
  clip-path: polygon(0 0, 95% 0, 100% 10%, 100% 100%, 5% 100%, 0 90%);
}

.option {
  padding: 0.8rem 1.2rem; color: #888;
  cursor: pointer; position: relative;
  display: flex; justify-content: space-between; align-items: center;
  transition: all 0.2s;
}

.optionHoverDecor {
  position: absolute; inset: 0; background: var(--c-neon);
  opacity: 0; transition: opacity 0.2s; z-index: 0;
}

.option:hover .optionHoverDecor { opacity: 0.15; }

.active { color: var(--c-neon); background: rgba(255,255,255,0.03); }

.optionText { position: relative; z-index: 1; font-weight: 600; }

.selectedTag {
  font-size: 0.6rem; background: var(--c-neon); color: #000;
  padding: 2px 4px; font-weight: 900;
}

.chevron { transition: transform 0.3s ease; color: var(--c-neon); }
.rotate { transform: rotate(180deg); filter: drop-shadow(0 0 5px var(--c-neon)); }
</style>