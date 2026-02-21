<template>
  <nav v-bind="$attrs" :class="$style.breadcrumbRoot" :style="breadcrumbStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index" :class="$style.item">
        <NuxtLink 
          :to="item.to" 
          :class="[$style.link, { [$style.active]: index === items.length - 1 }]"
        >
          <Icon v-if="item.icon" :name="item.icon" :class="$style.icon" />
          {{ item.label }}
        </NuxtLink>
        <span v-if="index < items.length - 1" :class="$style.separator">
          <slot name="separator"> / </slot>
        </span>
      </li>
    </ol>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

interface BreadcrumbItem {
  label: string;
  to: string;
  icon?: string;
}

const props = defineProps<{
  items: BreadcrumbItem[];
  color?: string;
  shadow?: string | boolean;
}>();

const breadcrumbStyles = computed(() => ({
  '--b-color': props.color || '#4f46e5',
  '--b-shadow': props.shadow === true ? "0 4px 12px rgba(0,0,0,0.1)" : (props.shadow || 'none')
}));
</script>

<style module>
.breadcrumbRoot {
  display: inline-block;
  padding: 0.5rem 1rem;
  background: #f3f4f6;
  clip-path: polygon(0% 0%, 95% 0%, 100% 50%, 95% 100%, 0% 100%);
  box-shadow: var(--b-shadow);
}

.list {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 0.5rem;
}

.link {
  text-decoration: none;
  color: #6b7280;
  font-weight: 600;
  font-size: 0.875rem;
  transition: color 0.2s;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.link:hover:not(.active) {
  color: var(--b-color);
}

.active {
  color: var(--b-color);
  pointer-events: none;
}

.separator {
  color: #d1d5db;
  font-weight: 300;
}
</style>