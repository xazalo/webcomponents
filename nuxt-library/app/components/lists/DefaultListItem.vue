<template>
  <li :class="[$style.item, { [$style.clickable]: clickable }]">
    <div v-if="$slots.icon" :class="$style.iconWrapper">
      <slot name="icon" />
    </div>
    
    <div :class="$style.content">
      <span :class="$style.label">{{ label }}</span>
      <span v-if="description" :class="$style.desc">{{ description }}</span>
    </div>

    <div v-if="$slots.suffix || value" :class="$style.suffix">
      <slot name="suffix">
        <span :class="$style.value">{{ value }}</span>
      </slot>
    </div>
  </li>
</template>

<script setup lang="ts">
defineProps<{
  label: string;
  description?: string;
  value?: string | number;
  clickable?: boolean;
}>();
</script>

<style module>
.item {
  display: flex;
  align-items: center;
  padding: 12px 8px;
  gap: 16px;
  transition: all 0.2s ease;
}

.clickable {
  cursor: pointer;
  border-radius: var(--list-item-radius);
}

.clickable:hover {
  background: var(--list-hover-bg);
  padding-left: 16px; /* Efecto sutil de desplazamiento */
}

.iconWrapper {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.label {
  font-weight: 700;
  font-size: 0.95rem;
}

.desc {
  font-size: 0.8rem;
  opacity: 0.6;
}

.value {
  font-weight: 800;
  color: var(--list-accent);
}
</style>