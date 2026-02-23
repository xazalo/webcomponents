<template>
  <div 
    :class="$style.ratingContainer" 
    :style="ratingStyles"
    role="img" 
    :aria-label="`Puntuación: ${modelValue} de ${maxStars}`"
  >
    <div :class="$style.starsWrapper">
      <span 
        v-for="star in maxStars" 
        :key="star"
        :class="[
          $style.star,
          { [$style.filled]: star <= Math.floor(modelValue) },
          { [$style.half]: isHalf(star) }
        ]"
      >
        ★
      </span>
    </div>

    <span v-if="showValue" :class="$style.valueLabel">
      {{ modelValue.toFixed(1) }}
    </span>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  modelValue: number;      // El valor del rating (ej: 4.5)
  maxStars?: number;       // Total de estrellas (default: 5)
  size?: string;           // Tamaño (ej: '1.5rem' o '24px')
  activeColor?: string;    // Color estrella encendida
  inactiveColor?: string;  // Color estrella apagada
  showValue?: boolean;     // Mostrar el número al lado
  gap?: string;            // Espacio entre estrellas
}

const props = withDefaults(defineProps<Props>(), {
  maxStars: 5,
  size: '1.25rem',
  activeColor: '#FFB800',
  inactiveColor: '#E2E8F0',
  showValue: false,
  gap: '4px'
});

const isHalf = (starIndex: number) => {
  return starIndex === Math.ceil(props.modelValue) && props.modelValue % 1 !== 0;
};

const ratingStyles = computed(() => ({
  '--star-size': props.size,
  '--star-active': props.activeColor,
  '--star-inactive': props.inactiveColor,
  '--star-gap': props.gap
}));
</script>

<style module>
.ratingContainer {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  line-height: 1;
}

.starsWrapper {
  display: flex;
  gap: var(--star-gap);
}

.star {
  position: relative;
  display: inline-block;
  font-size: var(--star-size);
  color: var(--star-inactive);
  user-select: none;
}

.filled {
  color: var(--star-active);
}

/* Soporte para medias estrellas */
.half::before {
  content: '★';
  position: absolute;
  left: 0;
  top: 0;
  width: 50%;
  overflow: hidden;
  color: var(--star-active);
}

.valueLabel {
  font-weight: 800;
  font-size: calc(var(--star-size) * 0.8);
  color: currentColor;
  opacity: 0.9;
}
</style>