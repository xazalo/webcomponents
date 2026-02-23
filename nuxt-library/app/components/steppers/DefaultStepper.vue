<template>
  <div 
    :class="[
      $style.stepperContainer, 
      $style[orientation],
      $style[variant]
    ]" 
    :style="stepperStyles"
  >
    <div 
      v-for="(step, index) in steps" 
      :key="index" 
      :class="[
        $style.stepWrapper,
        { [$style.active]: currentStep === index },
        { [$style.completed]: currentStep > index }
      ]"
    >
      <div v-if="index !== 0" :class="$style.connector"></div>

      <div :class="$style.stepNode">
        <div :class="$style.iconCircle">
          <transition name="scale" mode="out-in">
            <span v-if="currentStep > index" :key="'check'">✓</span>
            <span v-else :key="'number'">{{ index + 1 }}</span>
          </transition>
        </div>
        
        <div :class="$style.stepInfo">
          <h4 :class="$style.stepTitle">{{ step.title }}</h4>
          <p v-if="step.description" :class="$style.stepDesc">
            {{ step.description }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Step {
  title: string;
  description?: string;
}

interface Props {
  steps: Step[];
  currentStep: number;
  orientation?: 'vertical' | 'horizontal';
  variant?: 'modern' | 'cyber' | 'brutal' | 'glass';
  accentColor?: string;
  baseColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  variant: 'modern',
  accentColor: '#3b82f6',
  baseColor: '#e2e8f0',
  currentStep: 0
});

const stepperStyles = computed(() => ({
  '--step-accent': props.accentColor,
  '--step-base': props.baseColor,
}));
</script>

<style module>
.stepperContainer {
  display: flex;
  width: 100%;
  gap: 10px;
}

.stepWrapper {
  position: relative;
  display: flex;
  flex: 1;
}

/* --- ORIENTACIONES --- */
.vertical { flex-direction: column; gap: 0; }
.horizontal { flex-direction: row; align-items: flex-start; }

/* --- NODO Y CÍRCULO --- */
.stepNode {
  display: flex;
  align-items: center;
  gap: 16px;
  z-index: 2;
  padding: 12px 0;
}

.vertical .stepNode { padding: 20px 0; }

.iconCircle {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 0.85rem;
  background: var(--step-base);
  color: #64748b;
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  border: 2px solid transparent;
  flex-shrink: 0;
}

/* --- CONECTORES --- */
.connector {
  position: absolute;
  background: var(--step-base);
  transition: background 0.4s ease;
}

.vertical .connector {
  width: 2px;
  height: 100%;
  left: 15px;
  top: -50%;
}

.horizontal .connector {
  height: 2px;
  width: 100%;
  top: 28px;
  left: -50%;
}

/* --- ESTADOS --- */
.active .iconCircle {
  background: #fff;
  border-color: var(--step-accent);
  color: var(--step-accent);
  box-shadow: 0 0 15px color-mix(in srgb, var(--step-accent), transparent 70%);
}

.completed .iconCircle {
  background: var(--step-accent);
  color: #fff;
}

.completed .connector {
  background: var(--step-accent);
}

/* --- TEXTOS --- */
.stepInfo { display: flex; flex-direction: column; gap: 2px; }
.stepTitle { margin: 0; font-size: 0.95rem; font-weight: 700; color: #1e293b; }
.stepDesc { margin: 0; font-size: 0.8rem; color: #64748b; line-height: 1.3; }

/* --- VARIANTES DE ESTILO --- */

/* CYBER */
.cyber .iconCircle { border-radius: 2px; clip-path: polygon(20% 0%, 100% 0%, 100% 80%, 80% 100%, 0% 100%, 0% 20%); }
.cyber .stepTitle { text-transform: uppercase; letter-spacing: 1px; color: #fff; }
.cyber .stepDesc { font-family: 'JetBrains Mono', monospace; color: var(--step-accent); opacity: 0.7; }

/* BRUTAL */
.brutal .iconCircle { border: 3px solid #000; border-radius: 0; box-shadow: 4px 4px 0 #000; }
.brutal .connector { height: 4px; background: #000; }
.brutal .stepTitle { text-transform: uppercase; font-weight: 900; }

/* RESPONSIVE */
@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
  .horizontal .connector {
    width: 2px; height: 100%; left: 15px; top: -50%;
  }
}
</style>