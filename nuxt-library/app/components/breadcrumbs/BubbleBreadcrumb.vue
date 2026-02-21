<template>
  <nav v-bind="$attrs" :class="$style.bubbleNav" :style="bubbleStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink :to="item.to" :class="$style.bubbleLink">
          {{ item.label }}
        </NuxtLink>
      </li>
    </ol>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  items: any[];
  color?: string;
  shadow?: string | boolean;
}>();

const bubbleStyles = computed(() => ({
  '--b-color': props.color || '#4f46e5',
  '--b-shadow': props.shadow === true ? "0 5px 15px -3px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.bubbleNav {
  display: block;
}

.list {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  padding: 0;
}

.item {
  position: relative;
  display: flex;
  align-items: center;
}

.item:not(:last-child)::after {
  content: "→";
  position: absolute;
  right: -1.2rem;
  color: #d1d5db;
  font-weight: bold;
}

.bubbleLink {
  padding: 0.4rem 1rem;
  background: white;
  border: 2px solid #e5e7eb;
  border-radius: 20px 10px 20px 10px;
  text-decoration: none;
  color: #4b5563;
  font-weight: 700;
  font-size: 0.8rem;
  box-shadow: var(--b-shadow);
  transition: all 0.3s ease;
}

.bubbleLink:hover {
  border-color: var(--b-color);
  color: var(--b-color);
  border-radius: 10px 20px 10px 20px;
  transform: translateY(-2px);
}
</style>