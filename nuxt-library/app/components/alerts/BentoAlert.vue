<template>
  <div :class="[$style.bentoAlert, $style[type]]">
    <div :class="[$style.cell, $style.mainCell]">
      <div :class="$style.iconWrapper">
        <slot name="icon">
          <span :class="$style.defaultIcon">!</span>
        </slot>
      </div>
      <div :class="$style.headerText">
        <span :class="$style.label">{{ label }}</span>
        <h3 :class="$style.title">{{ title }}</h3>
      </div>
    </div>

    <div :class="[$style.cell, $style.contentCell]">
      <p :class="$style.description">{{ message }}</p>
    </div>

    <div :class="[$style.cell, $style.actionCell]" @click="$emit('action')">
      <span :class="$style.actionLabel">{{ actionText }}</span>
      <div :class="$style.arrow">→</div>
    </div>

    <button :class="[$style.cell, $style.closeCell]" @click="$emit('close')">
      ✕
    </button>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  title: string;
  message: string;
  type: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  label?: string
}>();

defineEmits(['close', 'action']);
</script>

<style module>
:root {
  --bento-black: #000000;
  --bento-white: #ffffff;
  --bento-shadow: 6px 6px 0px #000000;
  
  /* Colores por tipo */
  --color-info: #A2D2FF;
  --color-warning: #FFD6A5;
  --color-error: #FFADAD;
  --color-success: #CAFFBF;
}

.bentoAlert {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto auto;
  gap: 12px;
  max-width: 500px;
  padding: 12px;
  background: var(--bento-white);
  font-family: 'Arial Black', sans-serif;
  border: 4px solid var(--bento-black);
  box-shadow: 12px 12px 0px rgba(0,0,0,0.1);
}

.cell {
  background: var(--bento-white);
  border: 3px solid var(--bento-black);
  padding: 15px;
  box-shadow: var(--bento-shadow);
  display: flex;
  flex-direction: column;
  transition: transform 0.1s ease;
}

/* Celda Principal (Título e Icono) */
.mainCell {
  grid-column: span 3;
  flex-direction: row;
  align-items: center;
  gap: 15px;
  background: var(--current-color, var(--color-info));
}

.iconWrapper {
  width: 40px;
  height: 40px;
  background: var(--bento-black);
  color: var(--bento-white);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  font-size: 1.2rem;
}

.label {
  font-size: 0.6rem;
  color: var(--bento-black);
  opacity: 0.7;
}

.title {
  margin: 0;
  font-size: 1.2rem;
  text-transform: uppercase;
  line-height: 1;
}

/* Celda de Contenido */
.contentCell {
  grid-column: span 4;
  background: var(--bento-white);
}

.description {
  margin: 0;
  font-family: 'Arial', sans-serif;
  font-weight: 600;
  font-size: 0.9rem;
  line-height: 1.4;
  color: #333;
}

/* Celda de Acción */
.actionCell {
  grid-column: span 3;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  background: var(--bento-black);
  color: var(--bento-white);
}

.actionCell:hover {
  transform: translate(-2px, -2px);
  box-shadow: 8px 8px 0px var(--current-color);
}

.actionLabel {
  font-size: 0.8rem;
  letter-spacing: 1px;
}

/* Celda de Cierre */
.closeCell {
  grid-column: span 1;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 1.2rem;
}

.closeCell:hover {
  background: var(--bento-black);
  color: var(--bento-white);
}

/* Variantes de color */
.info { --current-color: var(--color-info); }
.warning { --current-color: var(--color-warning); }
.error { --current-color: var(--color-error); }
.success { --current-color: var(--color-success); }

@media (max-width: 400px) {
  .bentoAlert { grid-template-columns: repeat(2, 1fr); }
  .mainCell { grid-column: span 2; }
  .contentCell { grid-column: span 2; }
  .actionCell { grid-column: span 1; }
  .closeCell { grid-column: span 1; }
}
</style>