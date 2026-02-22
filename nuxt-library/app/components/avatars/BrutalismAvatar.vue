<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.brutalFrame">
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
  color: '#A3E635', // Un verde lima chillón muy Neo-Brutalista
  initials: '??'
});

const avatarStyles = computed(() => {
  const sizes = { 
    sm: '40px', 
    md: '60px', 
    lg: '90px', 
    xl: '130px' 
  };
  
  return {
    '--a-size': sizes[props.size],
    '--a-bg': props.color,
    // Bordes más rectos o con un redondeado muy leve
    '--a-radius': props.size === 'sm' ? '2px' : '4px',
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
  /* Espacio para la sombra rígida proyectada */
  margin-right: 6px;
  margin-bottom: 6px;
}

.brutalFrame {
  width: 100%;
  height: 100%;
  background: var(--a-bg);
  border-radius: var(--a-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
  
  /* CLAVE: Borde negro grueso y sombra sólida (offset) sin blur */
  border: 3px solid #000000;
  box-shadow: 5px 5px 0px 0px #000000;
  
  transition: all 0.1s ease-in-out;
}

.brutalFrame:active {
  /* Efecto de "presionar" el botón */
  transform: translate(3px, 3px);
  box-shadow: 2px 2px 0px 0px #000000;
}

.avatarImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  /* Quitamos el suavizado para que se vea más rudo */
  filter: contrast(1.1) saturate(1.2);
}

.initials {
  color: #000000;
  font-weight: 900;
  /* Tipografía grotesca o muy pesada */
  font-family: 'Arial Black', sans-serif; 
  font-size: calc(var(--a-size) * 0.35);
  text-transform: uppercase;
}

.statusBadge {
  position: absolute;
  bottom: -4px;
  right: -4px;
  width: 25%;
  height: 25%;
  border: 2px solid #000000;
  box-shadow: 2px 2px 0px 0px #000000;
  z-index: 2;
}

/* Colores ultra-saturados */
.online { background: #00FF00; }
.offline { background: #808080; }
.busy { background: #FF0000; }
</style>