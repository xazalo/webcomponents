<template>
  <div 
    :class="[
      $style.listContainer, 
      $style[gapSize]
    ]" 
    :style="listStyles"
  >
    <header v-if="title || $slots.header" :class="$style.listHeader">
      <slot name="header">
        <h3 :class="$style.title">{{ title }}</h3>
        <span v-if="subtitle" :class="$style.subtitle">{{ subtitle }}</span>
      </slot>
    </header>

    <ul :class="$style.listBody">
      <slot />
    </ul>

    <footer v-if="$slots.footer" :class="$style.listFooter">
      <slot name="footer" />
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  subtitle?: string;
  // Configuración
  gapSize?: 'none' | 'small' | 'medium' | 'large';
  divider?: boolean;
  // Estética (Variables que inyectamos)
  primaryColor?: string;
  hoverEffect?: boolean;
  padding?: string;
}

const props = withDefaults(defineProps<Props>(), {
  gapSize: 'medium',
  divider: true,
  primaryColor: '#3b82f6',
  hoverEffect: true,
  padding: '0px'
});

const listStyles = computed(() => ({
  '--list-accent': props.primaryColor,
  '--list-padding': props.padding,
  '--list-divider': props.divider ? '1px solid rgba(0,0,0,0.06)' : 'none',
  '--list-hover-bg': 'rgba(0,0,0,0.02)',
  '--list-item-radius': '12px'
}));
</script>

<style module>
.listContainer {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: var(--list-padding);
}

.listHeader {
  margin-bottom: 16px;
  padding: 0 8px;
}

.title {
  margin: 0;
  font-size: 1.1rem;
  font-weight: 800;
}

.subtitle {
  font-size: 0.85rem;
  opacity: 0.6;
}

.listBody {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
}

/* Gaps */
.none { gap: 0; }
.small { gap: 4px; }
.medium { gap: 12px; }
.large { gap: 20px; }

.listFooter {
  margin-top: 16px;
  padding-top: 12px;
  border-top: var(--list-divider);
}

/* Estilos para los Items (se aplican a lo que metas en el slot) */
.listBody > * {
  border-bottom: var(--list-divider);
}

.listBody > *:last-child {
  border-bottom: none;
}
</style>