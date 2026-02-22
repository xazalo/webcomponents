<template>
  <div :class="[$style.brutalAlert, $style[type]]">
    <div :class="$style.statusBadge">
      {{ type.toUpperCase() }}
    </div>

    <div :class="$style.headerBlock">
      <h3 :class="$style.title">{{ title }}</h3>
      <button :class="$style.closeBtn" @click="$emit('close')">{{ buttonText }}</button>
    </div>

    <div :class="$style.bodyBlock">
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footerBlock">
      <button :class="$style.mainAction" @click="$emit('action')">
        {{ actionText }} <span :class="$style.blink">_</span>
      </button>
      <div :class="$style.decorativePattern"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  title: string;
  message: string;
  type: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  buttonText?: string
}>();

defineEmits(['close', 'action']);
</script>

<style module>
.brutalAlert {
  --b-black: #000000;
  --b-white: #ffffff;
  /* Colores de alto contraste */
  --b-info: #0047ff;
  --b-warning: #ffcf00;
  --b-error: #ff0000;
  --b-success: #00ff41;
  
  display: flex;
  flex-direction: column;
  background: var(--b-white);
  border: 6px solid var(--b-black);
  box-shadow: 16px 16px 0px var(--b-black);
  max-width: 450px;
  position: relative;
  font-family: 'Impact', 'Arial Black', sans-serif;
}

/* Badge de estado flotante */
.statusBadge {
  position: absolute;
  top: -20px;
  left: 20px;
  background: var(--current-color);
  color: var(--b-black);
  padding: 4px 12px;
  border: 4px solid var(--b-black);
  font-size: 0.9rem;
  font-weight: 900;
  transform: rotate(-2deg);
}

.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 20px 10px;
  border-bottom: 6px solid var(--b-black);
  background: var(--b-black);
  color: var(--b-white);
}

.title {
  margin: 0;
  font-size: 1.8rem;
  letter-spacing: -1px;
  text-transform: uppercase;
}

.closeBtn {
  background: var(--b-white);
  border: 3px solid var(--b-black);
  font-family: monospace;
  font-weight: bold;
  cursor: pointer;
  padding: 4px 8px;
}

.closeBtn:hover {
  background: var(--b-error);
  color: var(--b-white);
}

.bodyBlock {
  padding: 20px;
  background: var(--b-white);
  border-bottom: 6px solid var(--b-black);
}

.message {
  margin: 0;
  font-family: 'Courier New', monospace;
  font-size: 1.1rem;
  font-weight: 900;
  color: var(--b-black);
  line-height: 1.2;
}

.footerBlock {
  display: flex;
  height: 60px;
}

.mainAction {
  flex: 2;
  background: var(--current-color);
  border: none;
  border-right: 6px solid var(--b-black);
  font-size: 1.2rem;
  font-weight: 900;
  text-transform: uppercase;
  cursor: pointer;
  padding: 0 20px;
  text-align: left;
}

.mainAction:hover {
  background: var(--b-black);
  color: var(--b-white);
}

.decorativePattern {
  flex: 1;
  background-color: var(--b-black);
  background-image: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 10px,
    var(--current-color) 10px,
    var(--current-color) 20px
  );
}

/* Variantes */
.info { --current-color: var(--b-info); }
.warning { --current-color: var(--b-warning); }
.error { --current-color: var(--b-error); }
.success { --current-color: var(--b-success); }

.blink {
  animation: blinker 0.8s linear infinite;
}

@keyframes blinker {
  50% { opacity: 0; }
}
</style>