<template>
  <nav :class="$style.bubbleMenu">
    <div :class="[$style.cell, $style.headerCell]">
      <div :class="$style.glare"></div>
      <span :class="$style.label">NAV</span>
      <h2 :class="$style.title">{{ activeLabel }}</h2>
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
        :style="{ '--accent-color': getBubbleColor(index) }"
        @click.prevent="$emit('update:modelValue', link.path)"
      >
        <div :class="$style.miniGlare"></div>
        <div :class="$style.content">
          <span :class="$style.linkTitle">{{ link.label }}</span>
          <span :class="$style.linkDesc">{{ link.description }}</span>
        </div>
        <div :class="$style.indicator">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="3"
          >
            <path d="M9 18l6-6-6-6" />
          </svg>
        </div>
      </a>
    </div>

    <button :class="[$style.cell, $style.logoutBtn]">
      {{ buttonText }}
    </button>
  </nav>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface NavLink {
  label: string;
  path: string;
  description?: string;
}

const props = defineProps<{
  modelValue: string;
  links: NavLink[];
  buttonText: string;
}>();

defineEmits(["update:modelValue"]);

const activeLabel = computed(() => {
  return props.links.find((l) => l.path === props.modelValue)?.label || "MENÚ";
});

const getBubbleColor = (index: number) => {
  const colors = ["#ff8dfb", "#70e0ff", "#8dffbe", "#ffdb70"];
  return colors[index % colors.length];
};
</script>

<style module>
:root {
  --bubble-white: #ffffff;
  --bubble-bg: #f0f4f8;
  --bubble-radius: 30px;
  /* Sombras para el efecto relieve/inflado */
  --bubble-soft-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
  --bubble-inner-light: inset 2px 4px 8px rgba(255, 255, 255, 0.8);
  --bubble-inner-dark: inset -2px -4px 8px rgba(0, 0, 0, 0.03);
}

.bubbleMenu {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 15px;
  background: var(--bubble-bg);
  border-radius: calc(var(--bubble-radius) + 10px);
  width: 100%;
  max-width: 400px;
  box-shadow: var(--bubble-soft-shadow);
}

.cell {
  background: var(--bubble-white);
  border-radius: var(--bubble-radius);
  padding: 16px 22px;
  border: none;
  position: relative;
  overflow: hidden;
  box-shadow: var(--bubble-inner-light), var(--bubble-inner-dark);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.headerCell {
  background: white;
  margin-bottom: 5px;
}

.label {
  font-size: 0.6rem;
  font-weight: 800;
  color: #adb5bd;
  letter-spacing: 1.5px;
  text-transform: uppercase;
}

.title {
  margin: 2px 0 0;
  font-size: 1.4rem;
  font-weight: 900;
  color: #2d3436;
}

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
  transform: scale(1.02);
  background: var(--accent-color);
}

.linkCell:hover .linkTitle,
.linkCell:hover .linkDesc,
.linkCell:hover .indicator {
  color: #fff;
}

.active {
  background: var(--accent-color) !important;
  transform: scale(0.98);
  box-shadow: inset 4px 4px 10px rgba(0, 0, 0, 0.1);
}

.active .linkTitle,
.active .linkDesc,
.active .indicator {
  color: #fff !important;
}

.linkTitle {
  display: block;
  font-weight: 800;
  font-size: 1.1rem;
  color: #495057;
}

.linkDesc {
  font-size: 0.75rem;
  font-weight: 600;
  color: #adb5bd;
}

.indicator {
  color: #dee2e6;
  transition: transform 0.3s ease;
}

.linkCell:hover .indicator {
  transform: translateX(4px);
}

/* Brillos Glossy */
.glare {
  position: absolute;
  top: 6px;
  left: 15%;
  width: 70%;
  height: 4px;
  background: rgba(255, 255, 255, 0.6);
  border-radius: 10px;
}

.miniGlare {
  position: absolute;
  top: 8px;
  left: 20px;
  width: 15px;
  height: 4px;
  background: rgba(255, 255, 255, 0.4);
  border-radius: 10px;
}

.logoutBtn {
  margin-top: 10px;
  background: #2d3436;
  color: white;
  font-weight: 800;
  font-size: 0.75rem;
  cursor: pointer;
  text-align: center;
}

.logoutBtn:active {
  transform: scale(0.95);
}
</style>
