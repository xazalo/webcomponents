<template>
  <div :class="[$style.neuAlert, $style[type]]">
    <div :class="$style.iconInverted">
      <div :class="$style.iconInner">
        <slot name="icon">!</slot>
      </div>
    </div>

    <div :class="$style.content">
      <span :class="$style.typeLabel">{{ type.toUpperCase() }}</span>
      <h3 :class="$style.title">{{ title }}</h3>
      <p :class="$style.message">{{ message }}</p>
    </div>

    <div :class="$style.footer">
      <button :class="$style.confirmBtn" @click="$emit('action')">
        {{ actionText }}
      </button>
      <button :class="$style.cancelBtn" @click="$emit('close')">
        {{ buttonText }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  title: string;
  message: string;
  type: 'info' | 'warning' | 'error' | 'success';
  actionText: string;
  buttonText: string;
}>();

defineEmits(['close', 'action']);
</script>

<style module>
:root {
  /* Color base crucial para el efecto */
  --neu-bg: #e0e5ec;
  --neu-light: #ffffff;
  --neu-shadow: #a3b1c6;
  
  /* Colores de acento sutiles */
  --neu-info: #6d5dfc;
  --neu-error: #ff7675;
  --neu-success: #55efc4;
  --neu-warning: #fab1a0;
}

.neuAlert {
  display: flex;
  flex-direction: column;
  padding: 35px;
  max-width: 420px;
  background: var(--neu-bg);
  border-radius: 50px;
  
  /* Sombras de extrusión: luz arriba-izquierda, sombra abajo-derecha */
  box-shadow: 
    9px 9px 16px var(--neu-shadow), 
    -9px -9px 16px var(--neu-light);
  
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  transition: all 0.3s ease;
}

.iconInverted {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  
  /* Efecto hundido (inset) */
  background: var(--neu-bg);
  box-shadow: 
    inset 6px 6px 10px var(--neu-shadow), 
    inset -6px -6px 10px var(--neu-light);
}

.iconInner {
  font-size: 1.5rem;
  font-weight: 900;
  color: var(--type-accent);
  /* Un pequeño resplandor para el icono */
  text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
}

.typeLabel {
  font-size: 0.7rem;
  font-weight: 800;
  color: #a3b1c6;
  letter-spacing: 1.5px;
}

.title {
  margin: 5px 0 12px;
  font-size: 1.6rem;
  color: #44475a;
  font-weight: 700;
}

.message {
  margin: 0 0 25px;
  font-size: 1rem;
  color: #7a8ba9;
  line-height: 1.6;
}

.footer {
  display: flex;
  gap: 20px;
}

.confirmBtn {
  flex: 1;
  padding: 15px;
  border-radius: 20px;
  border: none;
  background: var(--neu-bg);
  color: var(--type-accent);
  font-weight: 700;
  cursor: pointer;
  
  /* Botón con relieve */
  box-shadow: 
    5px 5px 10px var(--neu-shadow), 
    -5px -5px 10px var(--neu-light);
  transition: all 0.2s ease;
}

.confirmBtn:active {
  /* Al pulsar, se hunde */
  box-shadow: 
    inset 3px 3px 6px var(--neu-shadow), 
    inset -3px -3px 6px var(--neu-light);
  transform: scale(0.98);
}

.cancelBtn {
  flex: 1;
  padding: 15px;
  background: transparent;
  border: none;
  color: #7a8ba9;
  font-weight: 600;
  cursor: pointer;
}

.cancelBtn:hover {
  color: #44475a;
}

/* Variantes de color para el acento del texto/iconos */
.info { --type-accent: var(--neu-info); }
.warning { --type-accent: var(--neu-warning); }
.error { --type-accent: var(--neu-error); }
.success { --type-accent: var(--neu-success); }
</style>