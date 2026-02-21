<template>
  <nav v-bind="$attrs" :class="$style.glassNav" :style="glassStyles">
    <ol :class="$style.list">
      <li v-for="(item, index) in items" :key="index">
        <NuxtLink :to="item.to" :class="$style.link">
          {{ item.label }}
        </NuxtLink>
        <Icon v-if="index < items.length - 1" name="lucide:chevron-right" :class="$style.sep" />
      </li>
    </ol>
  </nav>
</template>

<script setup lang="ts">
import { computed } from 'vue';
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  items: any[];
  shadow?: string | boolean;
}>();

const glassStyles = computed(() => ({
  '--g-shadow': props.shadow === true ? "0 8px 32px 0 rgba(31, 38, 135, 0.15)" : (props.shadow || 'none')
}));
</script>

<style module>
.glassNav {
  display: inline-flex;
  padding: 0.6rem 1.2rem;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 50px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: var(--g-shadow);
}

.list {
  display: flex;
  align-items: center;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 0.8rem;
}

.list li {
  display: flex;
  align-items: center;
  gap: 0.8rem;
}

.link {
  text-decoration: none;
  color: #1f2937;
  font-size: 0.85rem;
  font-weight: 500;
  opacity: 0.7;
}

.link:hover { opacity: 1; }

.sep {
  width: 14px;
  color: rgba(0,0,0,0.3);
}
</style>