<template>
  <div :class="[$style.clayAlert, $style[type]]">
    <div :class="$style.iconBox">
      <slot name="icon">!</slot>
    </div>

    <div :class="$style.content">
      <h3 :class="$style.title">{{ title }}</h3>
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footer">
      <button :class="$style.mainBtn" @click="$emit('action')">
        {{ actionText }}
      </button>
      <button :class="$style.secondaryBtn" @click="$emit('close')">
        {{ buttonText }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  title: string;
  message: string;
  type: "info" | "warning" | "error" | "success";
  actionText: string;
  buttonText: string;
}>();

defineEmits(["close", "action"]);
</script>

<style module>
:root {
  --clay-white: #ffffff;
  --clay-text: #444b6e;
  /* Paleta Pastel Clay */
  --c-info: #85e3ff;
  --c-warning: #ffde91;
  --c-error: #ff9aa2;
  --c-success: #b2f2bb;
}

.clayAlert {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 30px;
  max-width: 420px;
  border-radius: 35px;
  background: var(--current-bg);

  /* El efecto Claymorphism real: sombras externas suaves + internas dobles */
  box-shadow:
    20px 20px 40px rgba(0, 0, 0, 0.08),
    inset -10px -10px 20px rgba(0, 0, 0, 0.1),
    inset 10px 10px 20px rgba(255, 255, 255, 0.4);

  font-family: "Quicksand", sans-serif;
  position: relative;
}

.iconBox {
  width: 50px;
  height: 50px;
  background: var(--current-bg);
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--clay-text);
  /* Sub-elemento también con efecto clay */
  box-shadow:
    8px 8px 16px rgba(0, 0, 0, 0.05),
    inset -4px -4px 8px rgba(0, 0, 0, 0.05),
    inset 4px 4px 8px rgba(255, 255, 255, 0.4);
}

.title {
  margin: 0 0 8px;
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--clay-text);
}

.message {
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
  color: rgba(68, 75, 110, 0.8);
  line-height: 1.5;
}

.footer {
  display: flex;
  gap: 12px;
}

.mainBtn {
  flex: 2;
  border: none;
  padding: 14px;
  border-radius: 20px;
  background: var(--clay-text);
  color: white;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease;
}

.mainBtn:hover {
  transform: translateY(-2px);
  filter: brightness(1.2);
}

.secondaryBtn {
  flex: 1;
  border: none;
  padding: 14px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.4);
  color: var(--clay-text);
  font-weight: 700;
  cursor: pointer;
  /* Efecto hundido para el botón secundario */
  box-shadow: inset 2px 2px 5px rgba(0, 0, 0, 0.05);
}

.secondaryBtn:hover {
  background: rgba(255, 255, 255, 0.6);
}

/* Variantes de color */
.info {
  --current-bg: var(--c-info);
}
.warning {
  --current-bg: var(--c-warning);
}
.error {
  --current-bg: var(--c-error);
}
.success {
  --current-bg: var(--c-success);
}
</style>
