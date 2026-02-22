<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.bentoFrame">
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
  color?: string; // Color de fondo si no hay imagen
  status?: 'online' | 'offline' | 'busy';
  shadow?: string | boolean;
}

const props = withDefaults(defineProps<Props>(), {
  size: 'md',
  color: '#f3f4f6', // Gris suave por defecto estilo Bento
  shadow: true,
  initials: '??'
});

const avatarStyles = computed(() => {
  const sizes = { 
    sm: '32px', 
    md: '48px', 
    lg: '80px', 
    xl: '120px' 
  };
  
  // Sombra suave y difusa típica de Bento UI
  const DEFAULT_SHADOW = "0 8px 20px -4px rgba(0,0,0,0.08), 0 4px 8px -4px rgba(0,0,0,0.04)";
  
  return {
    '--a-size': sizes[props.size],
    '--a-bg': props.color,
    '--a-radius': props.size === 'sm' ? '8px' : '16px', // Radio proporcional al tamaño
    '--a-shadow': typeof props.shadow === 'string' ? props.shadow : (props.shadow ? DEFAULT_SHADOW : 'none'),
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
  padding: 2px; /* Espacio para el borde/glow si se desea */
}

.bentoFrame {
  width: 100%;
  height: 100%;
  background: var(--a-bg);
  border-radius: var(--a-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  box-shadow: var(--a-shadow);
  border: 1px solid rgba(0, 0, 0, 0.05); /* Borde casi imperceptible */
  transition: transform 0.2s ease;
}

.bentoFrame:hover {
  transform: scale(1.02); /* Efecto sutil al pasar el ratón */
}

.avatarImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  /* Mantenemos el radio del contenedor */
  border-radius: var(--a-radius);
}

.initials {
  color: #374151; /* Texto oscuro para contraste en fondo claro */
  font-weight: 600;
  font-family: sans-serif;
  font-size: calc(var(--a-size) * 0.35);
  text-transform: uppercase;
}

.statusBadge {
  position: absolute;
  bottom: -2px;
  right: -2px;
  width: 22%;
  height: 22%;
  border-radius: 35%;
  border: 2px solid #ffffff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.online { background: #22c55e; }
.offline { background: #94a3b8; }
.busy { background: #f43f5e; }
</style>