<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>THE_CAROUSEL_ARCHIVE</h1>
      <p>MULTI_STYLE_UI_SYSTEM // v2026.02</p>
    </header>

    <main :class="$style.main">
      
      <section :class="$style.displaySection">
        <div :class="$style.info">
          <span :class="$style.tag">Bento UI</span>
          <h3>Modern Grid System</h3>
        </div>
        <BentoCarousel :items="mediaItems" color="#6366f1" gap="20px" />
      </section>

      <section :class="[$style.displaySection, $style.bubbleBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.bubbleTag]">Bubble UI</span>
          <h3>Soft & Elastic</h3>
        </div>
        <BubbleCarousel :items="mediaItems" color="#ff9ff3" gap="24px" />
      </section>

      <section :class="[$style.displaySection, $style.clayBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.clayTag]">Claymorphism</span>
          <h3>Plastic Texture</h3>
        </div>
        <ClayCarousel :items="mediaItems" color="#48dbfb" gap="40px" />
      </section>

      <section :class="[$style.displaySection, $style.glassBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.glassTag]">Glassmorphism</span>
          <h3 style="color: white">Frosted Layers</h3>
        </div>
        <GlassCarousel :items="mediaItems" color="rgba(255,255,255,0.5)" gap="24px" />
      </section>

      <section :class="[$style.displaySection, $style.cyberBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.cyberTag]">Cyberpunk</span>
          <h3 :class="$style.cyberTitle">Digital Terminal</h3>
        </div>
        <CyberCarousel :items="mediaItems" color="#00f3ff" gap="15px" />
      </section>

      <section :class="[$style.displaySection, $style.neuBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.neuTag]">Soft UI</span>
          <h3>Neumorphic Extrusion</h3>
        </div>
        <NeuCarousel :items="mediaItems" color="#e0e5ec" gap="30px" />
      </section>

      <section :class="[$style.displaySection, $style.speedBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.speedTag]">Speed Style</span>
          <h3 style="color: white">Turbo Interface</h3>
        </div>
        <SpeedCarousel :items="mediaItems" color="#ff003c" gap="10px" />
      </section>

      <section :class="[$style.displaySection, $style.brutalBg]">
        <div :class="$style.info">
          <span :class="[$style.tag, $style.brutalTag]">Neobrutalism</span>
          <h3 style="font-family: 'Arial Black'">Raw Geometry</h3>
        </div>
        <BrutalismCarousel :items="mediaItems" color="#fcee0a" gap="30px" />
      </section>

    </main>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoCarousel, 
  BrutalismCarousel, 
  BubbleCarousel, 
  ClayCarousel, 
  CyberCarousel, 
  GlassCarousel, 
  NeuCarousel, 
  SpeedCarousel 
} from "../components/carousels";

const mediaItems = [
  { url: "https://images.unsplash.com/photo-1614850523296-d8c1af93d400?auto=format&fit=crop&w=800", aspectRatio: "9/16", label: "REEL_01" },
  { url: "https://www.w3schools.com/html/mov_bbb.mp4", aspectRatio: "21/9", label: "CINEMA_02" },
  { url: "https://images.unsplash.com/photo-1633167606207-d840b5070fc2?auto=format&fit=crop&w=800", aspectRatio: "1/1", label: "SQUARE_03" },
  { url: "https://images.unsplash.com/photo-1550745165-9bc0b252726f?auto=format&fit=crop&w=800", aspectRatio: "16/9", label: "WIDE_04" },
];
</script>

<style module>
/* --- Layout Base --- */
.showroomContainer {
  min-height: 100vh;
  padding: 100px 20px;
  background-color: #f8fafc;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header { text-align: center; margin-bottom: 80px; }
.header h1 { font-size: 3rem; font-weight: 900; letter-spacing: -3px; color: #0f172a; }

.main {
  width: 100%;
  max-width: 1200px;
  display: flex;
  flex-direction: column;
  gap: 100px;
}

.displaySection {
  background: white;
  padding: 60px 40px;
  border-radius: 48px;
  border: 1px solid rgba(0,0,0,0.05);
  box-shadow: 0 30px 60px -20px rgba(0,0,0,0.05);
}

.info { margin-bottom: 40px; }
.info h3 { font-size: 1.8rem; font-weight: 800; margin-top: 10px; color: #1e293b; }
.tag { font-size: 0.7rem; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; padding: 4px 12px; border-radius: 8px; background: #f1f5f9; color: #64748b; }

/* --- Variantes de Contexto --- */

/* Bubble: Fondo pastel suave */
.bubbleBg { background: #fff5f9; border-color: #ffdeeb; }
.bubbleTag { background: #ff9ff3; color: white; border-radius: 30px; }

/* Clay: Fondo color crema/arcilla */
.clayBg { background: #f0f7ff; }
.clayTag { background: #48dbfb; color: white; border-radius: 20px; }

/* Glass: Gradiente vibrante */
.glassBg { 
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
  position: relative; overflow: hidden;
}
.glassTag { background: rgba(255,255,255,0.2); color: white; backdrop-filter: blur(5px); }

/* Cyber: Negro total */
.cyberBg { background: #050505; border-color: #1a1a1a; }
.cyberTitle { color: #00f3ff; font-family: monospace; text-transform: uppercase; }
.cyberTag { background: #00f3ff; color: black; border-radius: 0; }

/* Neumorphism: Gris específico */
.neuBg { background: #e0e5ec; border: none; box-shadow: none; }
.neuTag { background: #e0e5ec; box-shadow: 3px 3px 6px #a3b1c6, -3px -3px 6px #fff; }

/* Speed: Asfalto */
.speedBg { background: #0f0f0f; border-color: #222; }
.speedTag { background: #ff003c; color: white; font-style: italic; }

/* Brutalism: Blanco con sombra dura */
.brutalBg { 
  border: 4px solid #000; 
  border-radius: 0; 
  box-shadow: 15px 15px 0px #000; 
}
.brutalTag { background: #000; color: #fff; border-radius: 0; }

@media (max-width: 768px) {
  .displaySection { padding: 30px 20px; }
}
</style>