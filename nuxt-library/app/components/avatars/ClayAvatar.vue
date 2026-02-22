<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.clayFrame">
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
  color?: string; 
  status?: 'online' | 'offline' | 'busy';
}

const props = withDefaults(defineProps<Props>(), {
  size: 'md',
  color: '#e0e7ff', // Un color pastel (lavanda) queda mejor en claymorphism
  initials: '??'
});

const avatarStyles = computed(() => {
  const sizes = { 
    sm: '40px', 
    md: '64px', 
    lg: '96px', 
    xl: '140px' 
  };
  
  return {
    '--a-size': sizes[props.size],
    '--a-bg': props.color,
    // Radio muy grande para el efecto "bubble"
    '--a-radius': props.size === 'sm' ? '12px' : '32px',
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
  /* El claymorphism necesita aire alrededor por sus sombras grandes */
  margin: 10px; 
}

.clayFrame {
  width: 100%;
  height: 100%;
  background: var(--a-bg);
  border-radius: var(--a-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
  
  /* LA MAGIA DEL CLAYMORPHISM: Sombras internas para el volumen */
  box-shadow: 
    /* Sombra externa (flotación) */
    0 16px 32px -8px rgba(0, 0, 0, 0.15),
    /* Sombra interna superior (brillo/highlight) */
    inset 0 4px 12px rgba(255, 255, 255, 0.8),
    /* Sombra interna inferior (profundidad) */
    inset 0 -4px 12px rgba(0, 0, 0, 0.1),
    /* Segunda sombra interna para acentuar el borde inferior */
    inset 0 -8px 24px rgba(0, 0, 0, 0.05);

  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.clayFrame:hover {
  transform: translateY(-4px) scale(1.02);
  box-shadow: 
    0 24px 48px -12px rgba(0, 0, 0, 0.2),
    inset 0 4px 12px rgba(255, 255, 255, 0.9),
    inset 0 -4px 12px rgba(0, 0, 0, 0.15);
}

.avatarImage {
  width: 85%; /* Un poco más pequeña para que se vea el marco clay */
  height: 85%;
  object-fit: cover;
  border-radius: calc(var(--a-radius) * 0.8);
  /* Sombra interna para la imagen para que parezca hundida */
  box-shadow: inset 0 2px 5px rgba(0,0,0,0.2);
}

.initials {
  color: #475569;
  font-weight: 800;
  font-family: 'Rounded', sans-serif; /* Tipografía redonda ayuda al estilo */
  font-size: calc(var(--a-size) * 0.3);
  text-shadow: 0 2px 4px rgba(255,255,255,0.5);
}

.statusBadge {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 28%;
  height: 28%;
  border-radius: 50%;
  border: 3px solid #ffffff;
  /* Badge también con estilo clay */
  box-shadow: 
    0 4px 8px rgba(0,0,0,0.1),
    inset 0 2px 4px rgba(255,255,255,0.5),
    inset 0 -2px 4px rgba(0,0,0,0.1);
}

.online { background: #4ade80; }
.offline { background: #cbd5e1; }
.busy { background: #fb7185; }
</style>