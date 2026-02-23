<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <span :class="$style.superTitle">COMPONENTS_V2</span>
      <h1 :class="$style.mainTitle">BENTO_COLLECTION</h1>
      <p :class="$style.subtitle">8_STYLES // DUAL_ORIENTATION // CONTEXT_AWARE</p>
    </header>

    <div :class="$style.masterGrid">
      
      <section :class="[$style.styleSection, $style.bgModern]">
        <h2 :class="$style.sectionTag">01. Bento Modern // Apple Style</h2>
        <div :class="$style.cardRow">
          <BentoCard title="Core Systems" description="Estructura base con bordes suaves." tag="Standard" image="https://picsum.photos/400/300?1" />
          <BentoCard title="Wide Intelligence" orientation="horizontal" tag="Insight" image="https://picsum.photos/600/400?1" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgBrutal]">
        <h2 :class="$style.sectionTag">02. Neo-Brutalism // High Contrast</h2>
        <div :class="$style.cardRow">
          <BrutalismCard title="Raw Power" tag="Execute" image="https://picsum.photos/400/300?2" primary-color="#000" accent-color="#fcee0a" />
          <BrutalismCard title="Direct Access" orientation="horizontal" tag="Admin" image="https://picsum.photos/600/400?2" primary-color="#000" accent-color="#00ff00" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgBubble]">
        <h2 :class="$style.sectionTag">03. Bubble Style // Soft & Playful</h2>
        <div :class="$style.cardRow">
          <BubbleCard title="Cute Flow" tag="Friendly" image="https://picsum.photos/400/300?3" primary-color="#ff85a1" />
          <BubbleCard title="Soft Experience" orientation="horizontal" tag="Cloud" image="https://picsum.photos/600/400?3" primary-color="#4ecdc4" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgClay]">
        <h2 :class="$style.sectionTag">04. Claymorphism // 3D Plastic</h2>
        <div :class="$style.cardRow">
          <ClayCard title="Volumetric" tag="Clay" image="https://picsum.photos/400/300?4" clay-color="#818cf8" text-color="#fff" />
          <ClayCard title="Molded Layout" orientation="horizontal" tag="Surface" image="https://picsum.photos/600/400?4" clay-color="#ffffff" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgCyber]">
        <h2 :class="$style.sectionTag">05. Cyberpunk // Neural Link</h2>
        <div :class="$style.cardRow">
          <CyberCard title="Initialize" tag="Protocol" image="https://picsum.photos/400/300?5" neon-color="#00f2fe" />
          <CyberCard title="Neural Network" orientation="horizontal" tag="Secure" image="https://picsum.photos/600/400?5" neon-color="#ff0055" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgGlass]">
        <h2 :class="$style.sectionTag">06. Glassmorphism // Crystal Layer</h2>
        <div :class="$style.cardRow">
          <GlassCard title="Frosted View" tag="Glass" image="https://picsum.photos/400/300?6" />
          <GlassCard title="Prism Effect" orientation="horizontal" tag="Clear" image="https://picsum.photos/600/400?6" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgNeu]">
        <h2 :class="$style.sectionTag">07. Neumorphism // Extruded Soft</h2>
        <div :class="$style.cardRow">
          <NeuCard title="Soft UI" tag="Neu" image="https://picsum.photos/400/300?7" />
          <NeuCard title="Tactile Feel" orientation="horizontal" tag="Plastic" image="https://picsum.photos/600/400?7" />
        </div>
      </section>

      <section :class="[$style.styleSection, $style.bgSpeed]">
        <h2 :class="$style.sectionTag">08. Speed Style // Racing HUD</h2>
        <div :class="$style.cardRow">
          <SpeedCard title="Full Nitro" tag="Fast" image="https://picsum.photos/400/300?8" accent-color="#fcee0a" />
          <SpeedCard title="Aerodynamic" orientation="horizontal" tag="Boost" image="https://picsum.photos/600/400?8" accent-color="#ff3e3e" />
        </div>
      </section>

    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoCard, BrutalismCard, BubbleCard, ClayCard, 
  CyberCard, GlassCard, NeuCard, SpeedCard 
} from "../components/cards";
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 60px 20px;
  background-color: #0f172a; /* Fondo base oscuro para que todo resalte */
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 80px;
  color: white;
}

.mainTitle { font-size: clamp(2rem, 6vw, 3.5rem); font-weight: 900; letter-spacing: -2px; }

/* Estructura de Secciones */
.masterGrid {
  max-width: 1300px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 60px;
}

.styleSection {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding: 60px 40px;
  border-radius: 40px;
  transition: transform 0.3s ease;
}

.sectionTag {
  font-size: 0.85rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 10px;
}

.cardRow {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 32px;
}

/* --- VARIANTES DE COLOR POR ESTILO --- */

.bgModern { background-color: #f1f5f9; color: #1e293b; }

.bgBrutal { background-color: #ffdeeb; color: #000; } /* Rosa pastel para brutalismo */

.bgBubble { background-color: #fff5f5; color: #ff85a1; }

.bgClay   { background-color: #e0f2fe; color: #0369a1; }

.bgCyber  { background-color: #050505; color: #00f2fe; border: 1px solid #111; }

.bgGlass  { 
  background: linear-gradient(45deg, #4f46e5, #ec4899); 
  color: white; 
}

.bgNeu    { background-color: #e0e5ec; color: #444; }

.bgSpeed  { 
  background: radial-gradient(circle at top right, #2a2a2a, #000); 
  color: #fcee0a; 
}

/* Responsive */
@media (max-width: 1024px) {
  .cardRow { grid-template-columns: 1fr; }
  .styleSection { padding: 30px 20px; }
}
</style>