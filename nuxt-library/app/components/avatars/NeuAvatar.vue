<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.neuFrame">
      <div :class="$style.innerBezel">
        <NuxtImg
          v-if="src"
          :src="src"
          :alt="alt"
          :class="$style.avatarImage"
          provider="ipx"
          loading="lazy"
        />
        <div v-else :class="$style.initials">{{ initials }}</div>
      </div>
    </div>
    
    <div v-if="status" :class="[$style.statusBadge, $style[status]]"></div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

defineOptions({ inheritAttrs: false });

interface Props {
  src?: string;
  alt?: string;
  initials?: string;
  size?: 'sm' | 'md' | 'lg' | 'xl';
  color?: string; // Debe ser el mismo color que el fondo de tu web
  status?: 'online' | 'offline' | 'busy';
}

const props = withDefaults(defineProps<Props>(), {
  size: 'md',
  color: '#e0e5ec', // Gris azulado clásico de Neumorphism
  initials: '??'
});

const avatarStyles = computed(() => {
  const sizes = { 
    sm: '48px', 
    md: '72px', 
    lg: '100px', 
    xl: '150px' 
  };
  
  return {
    '--a-size': sizes[props.size],
    '--a-bg': props.color,
    '--a-radius': '25%', // El neumorfismo prefiere formas orgánicas suaves
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
  padding: 10px; /* Espacio para que respiren las sombras */
}

.neuFrame {
  width: 100%;
  height: 100%;
  background: var(--a-bg);
  border-radius: var(--a-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  
  /* SOMBRAS NEUMÓRFICAS: Una clara (luz) y una oscura (sombra) */
  box-shadow: 
    9px 9px 16px rgba(163, 177, 198, 0.6), 
    -9px -9px 16px rgba(255, 255, 255, 0.5);
  
  transition: all 0.2s ease;
  cursor: pointer;
}

.neuFrame:active {
  /* Efecto de hundido al presionar */
  box-shadow: 
    inset 4px 4px 8px rgba(163, 177, 198, 0.6), 
    inset -4px -4px 8px rgba(255, 255, 255, 0.5);
}

.innerBezel {
  width: 88%;
  height: 88%;
  border-radius: var(--a-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  /* Borde sutil para definir el área de la imagen */
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.avatarImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.initials {
  color: #748194;
  font-weight: 600;
  font-family: 'Segoe UI', sans-serif;
  font-size: calc(var(--a-size) * 0.3);
}

.statusBadge {
  position: absolute;
  bottom: 12%;
  right: 12%;
  width: 20%;
  height: 20%;
  border-radius: 50%;
  border: 3px solid var(--a-bg);
  /* El badge también tiene relieve */
  box-shadow: 
    2px 2px 4px rgba(163, 177, 198, 0.6),
    -1px -1px 2px rgba(255, 255, 255, 0.5);
}

.online { background: #4ade80; }
.offline { background: #94a3b8; }
.busy { background: #f87171; }
</style>