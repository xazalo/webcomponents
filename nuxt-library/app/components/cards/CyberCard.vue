<template>
  <div 
    :class="[
      $style.cyberCard, 
      $style[orientation], 
      { [$style.hasImage]: !!image }
    ]" 
    :style="cardStyles"
  >
    <div :class="$style.cornerTrim"></div>
    <div :class="$style.scanline"></div>

    <div v-if="image" :class="$style.imageWrapper">
      <div :class="$style.imageGlitch"></div>
      <img :src="image" :alt="title" :class="$style.image" loading="lazy" />
      <div v-if="$slots.badge" :class="$style.imageBadge">
        <slot name="badge" />
      </div>
    </div>

    <div :class="$style.content">
      <header :class="$style.header">
        <div :class="$style.tagRow">
          <span v-if="tag" :class="$style.tag">{{ tag }}</span>
          <span :class="$style.serialNumber">#{{ Math.floor(Math.random()*9000) }}</span>
        </div>
        <h3 :class="$style.title" :data-text="title">{{ title }}</h3>
        <p v-if="description" :class="$style.description">{{ description }}</p>
      </header>

      <div :class="$style.body">
        <slot />
      </div>

      <footer v-if="$slots.footer" :class="$style.footer">
        <div :class="$style.footerLine"></div>
        <slot name="footer" />
      </footer>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title: string;
  description?: string;
  tag?: string;
  image?: string;
  orientation?: 'vertical' | 'horizontal';
  align?: 'start' | 'center' | 'end';
  // Personalización Cyber
  neonColor?: string;     // Color del neón principal (Cyan, Magenta, etc.)
  bgCard?: string;        // Fondo oscuro
  glitchEffect?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  orientation: 'vertical',
  align: 'start',
  neonColor: '#00f2fe',
  bgCard: '#0a0a0b',
  glitchEffect: true
});

const cardStyles = computed(() => ({
  '--cyber-neon': props.neonColor,
  '--cyber-bg': props.bgCard,
  '--cyber-align': props.align === 'center' ? 'center' : props.align === 'end' ? 'flex-end' : 'flex-start',
  '--cyber-text-align': props.align,
  '--cyber-glow': `0 0 15px ${props.neonColor}44`,
}));
</script>

<style module>
.cyberCard {
  background: var(--cyber-bg);
  color: #fff;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  height: 100%;
  border: 1px solid color-mix(in srgb, var(--cyber-neon), transparent 70%);
  /* Corte angular estilo Cyberpunk */
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 20px), calc(100% - 20px) 100%, 0 100%);
  transition: all 0.3s ease;
  text-align: var(--cyber-text-align);
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
}

.cyberCard:hover {
  border-color: var(--cyber-neon);
  box-shadow: var(--cyber-glow);
  transform: scale(1.01);
}

/* --- DECORACIONES --- */
.cornerTrim {
  position: absolute;
  top: 0;
  right: 0;
  width: 40px;
  height: 4px;
  background: var(--cyber-neon);
  box-shadow: 0 0 10px var(--cyber-neon);
}

.scanline {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: rgba(255, 255, 255, 0.05);
  z-index: 10;
  pointer-events: none;
  animation: scan 4s linear infinite;
}

/* --- MEDIA --- */
.imageWrapper {
  position: relative;
  flex-shrink: 0;
  background: #000;
  border-bottom: 2px solid var(--cyber-neon);
}

.vertical .imageWrapper { height: 180px; }
.horizontal .imageWrapper { width: 40%; border-bottom: none; border-right: 2px solid var(--cyber-neon); }

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.7;
  filter: saturate(1.5) contrast(1.2);
  transition: opacity 0.3s;
}

.cyberCard:hover .image {
  opacity: 1;
}

.imageBadge {
  position: absolute;
  bottom: 10px;
  right: 10px;
  background: var(--cyber-neon);
  color: #000;
  font-weight: 900;
  font-size: 0.65rem;
  padding: 2px 8px;
  text-transform: uppercase;
}

/* --- CONTENIDO --- */
.content {
  padding: 25px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  flex: 1;
}

.tagRow {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tag {
  color: var(--cyber-neon);
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.serialNumber {
  font-size: 0.6rem;
  opacity: 0.3;
}

.title {
  margin: 0;
  font-size: 1.6rem;
  font-weight: 900;
  text-transform: uppercase;
  color: #fff;
  position: relative;
}

/* Efecto Hover en Título */
.cyberCard:hover .title::before {
  content: attr(data-text);
  position: absolute;
  left: 2px;
  text-shadow: -1px 0 #ff0055;
  background: var(--cyber-bg);
  overflow: hidden;
  animation: glitch 0.3s infinite;
}

.description {
  margin: 0;
  font-size: 0.85rem;
  line-height: 1.4;
  color: #a0a0a0;
  border-left: 2px solid rgba(255,255,255,0.1);
  padding-left: 10px;
}

.footerLine {
  height: 1px;
  background: linear-gradient(90deg, var(--cyber-neon), transparent);
  margin-bottom: 15px;
}

/* --- ANIMACIONES --- */
@keyframes scan {
  0% { top: -10%; }
  100% { top: 110%; }
}

@keyframes glitch {
  0% { clip-path: inset(20% 0 30% 0); }
  20% { clip-path: inset(60% 0 10% 0); }
  40% { clip-path: inset(40% 0 50% 0); }
  60% { clip-path: inset(80% 0 5% 0); }
  100% { clip-path: inset(10% 0 70% 0); }
}

@media (max-width: 768px) {
  .horizontal { flex-direction: column; }
  .horizontal .imageWrapper { width: 100%; border-right: none; border-bottom: 2px solid var(--cyber-neon); }
}
</style>