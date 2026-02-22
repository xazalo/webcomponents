<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.speedLine"></div>
    <div :class="$style.speedLineSecondary"></div>
    
    <div :class="$style.speedFrame">
      <NuxtImg
        v-if="src"
        :src="src"
        :alt="alt"
        :class="$style.avatarImage"
        provider="ipx"
        loading="lazy"
      />
      <div v-else :class="$style.initials">{{ initials }}</div>
      
      <div :class="$style.glare"></div>
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
  color: '#0066ff', // Azul "Racing"
  initials: 'V8'
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
    '--a-color': props.color,
    '--a-skew': '-12deg', // La clave de la velocidad es la inclinación
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
  margin: 0 15px;
}

.speedFrame {
  width: 100%;
  height: 100%;
  background: #111;
  border: 2px solid var(--a-color);
  transform: skewX(var(--a-skew));
  overflow: hidden;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 10px 0 20px -5px rgba(var(--a-color), 0.3);
  z-index: 2;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.avatarImage {
  width: 120%; /* Más ancho para compensar el skew */
  height: 100%;
  object-fit: cover;
  transform: skewX(calc(var(--a-skew) * -1)); /* Des-inclina la foto para que no se vea deformada */
  filter: saturate(1.2) contrast(1.1);
}

.initials {
  color: white;
  font-weight: 900;
  font-style: italic;
  font-family: 'Inter', sans-serif;
  font-size: calc(var(--a-size) * 0.35);
  transform: skewX(calc(var(--a-skew) * -1));
  text-shadow: 2px 2px 0px var(--a-color);
}

/* Efecto de líneas de velocidad */
.speedLine, .speedLineSecondary {
  position: absolute;
  top: 50%;
  right: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--a-color));
  border-radius: 2px;
  z-index: 1;
}

.speedLine {
  width: 40px;
  transform: translateY(-8px);
  opacity: 0.6;
}

.speedLineSecondary {
  width: 25px;
  transform: translateY(8px);
  opacity: 0.4;
}

/* Reflejo dinámico */
.glare {
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.4),
    transparent
  );
  transform: skewX(var(--a-skew));
}

.avatarRoot:hover .speedFrame {
  transform: skewX(var(--a-skew)) translateX(5px);
  box-shadow: 15px 0 30px -5px var(--a-color);
}

.avatarRoot:hover .glare {
  animation: shine 0.6s forwards;
}

@keyframes shine {
  100% { left: 150%; }
}

.statusBadge {
  position: absolute;
  bottom: -2px;
  right: -5px;
  width: 24%;
  height: 24%;
  transform: skewX(var(--a-skew));
  border: 2px solid #000;
  z-index: 3;
}

.online { background: #00ffcc; box-shadow: 0 0 10px #00ffcc; }
.offline { background: #444; }
.busy { background: #ff0055; box-shadow: 0 0 10px #ff0055; }

</style>