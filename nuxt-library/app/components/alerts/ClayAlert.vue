<template>
  <div :class="[$style.clayAlert, $style[type]]" :style="dynamicStyles">
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
import { computed } from 'vue';

interface Props {
  title: string;
  message: string;
  type: "info" | "warning" | "error" | "success";
  actionText: string;
  buttonText: string;
  // Dynamic color overrides
  clayColor?: string;
  textColor?: string;
  btnBg?: string;
}

const props = withDefaults(defineProps<Props>(), {
  textColor: '#444b6e',
  btnBg: '#444b6e'
});

defineEmits(["close", "action"]);

const dynamicStyles = computed(() => {
  // Default Pastel Clay Palette
  const typeColors = {
    info: '#85e3ff',
    warning: '#ffde91',
    error: '#ff9aa2',
    success: '#b2f2bb'
  };

  return {
    '--current-bg': props.clayColor || typeColors[props.type],
    '--clay-text': props.textColor,
    '--btn-main-bg': props.btnBg,
  };
});
</script>

<style module>
.clayAlert {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 30px;
  max-width: 420px;
  border-radius: 35px;
  background: var(--current-bg);

  /* The Claymorphism Effect */
  box-shadow:
    20px 20px 40px rgba(0, 0, 0, 0.08),
    inset -10px -10px 20px rgba(0, 0, 0, 0.1),
    inset 10px 10px 20px rgba(255, 255, 255, 0.4);

  font-family: "Quicksand", sans-serif;
  position: relative;
  color: var(--clay-text);
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
  
  /* Clay effect on sub-element */
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
  /* Using opacity on the text color variable for hierarchy */
  color: var(--clay-text);
  opacity: 0.8;
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
  background: var(--btn-main-bg);
  color: #ffffff;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, filter 0.2s ease;
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
  /* Sunken "inset" shadow for the secondary button */
  box-shadow: inset 2px 2px 5px rgba(0, 0, 0, 0.05);
  transition: background 0.2s ease;
}

.secondaryBtn:hover {
  background: rgba(255, 255, 255, 0.6);
}
</style>