<template>
  <span v-bind="$attrs" :class="$style.speedLabel" :style="speedStyles">
    <div :class="$style.glitch"></div>
    <span :class="$style.content">
      <slot />
    </span>
  </span>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  color?: string;
}>();

const speedStyles = computed(() => ({
  '--s-accent': props.color || '#00ff41', // Verde "hacker/racing"
  '--s-skew': '-20deg',
}));
</script>

<style module>
.speedLabel {
  display: inline-flex;
  align-items: center;
  padding: 0.2rem 1rem;
  font-size: 0.7rem;
  font-weight: 900;
  font-style: italic;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #fff;
  background: #000;
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
  position: relative;
  overflow: hidden;
  
  /* Aerodinámica */
  transform: skewX(var(--s-skew));
  border-right: 3px solid var(--s-accent);
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
}

.content {
  /* Corregimos el texto para que sea legible pero mantenga el ángulo */
  display: block;
  transform: skewX(5deg);
  z-index: 2;
  text-shadow: 2px 2px 0px rgba(0, 0, 0, 0.5);
}

.speedLabel::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg, 
    transparent, 
    rgba(255, 255, 255, 0.2), 
    transparent
  );
  transition: left 0.3s ease;
}

.speedLabel:hover {
  background: #111;
  transform: skewX(var(--s-skew)) translateX(8px);
  box-shadow: -5px 0 15px var(--s-accent);
}

.speedLabel:hover::before {
  left: 100%;
}

.speedLabel:hover .content {
  animation: vibration 0.1s infinite;
}

@keyframes vibration {
  0% { transform: skewX(5deg) translate(0); }
  25% { transform: skewX(5deg) translate(1px, -1px); }
  50% { transform: skewX(5deg) translate(-1px, 1px); }
  75% { transform: skewX(5deg) translate(1px, 1px); }
  100% { transform: skewX(5deg) translate(0); }
}
</style>