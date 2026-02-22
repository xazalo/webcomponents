<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1 :class="$style.glitch" data-text="ACTION_TRIGGERS">ACTION_TRIGGERS</h1>
      <p>INTERACTIVE_ELEMENTS // MULTI-STYLE_RESOURCES</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Soft</span>
        <div :class="$style.buttonRow">
          <BentoButton>Action Primary</BentoButton>
          <BentoButton variant="outline" color="#10b981">Success Outline</BentoButton>
          <BentoButton size="sm" icon="lucide:settings" color="#6b7280" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // High Contrast</span>
        <div :class="$style.buttonRow">
          <BrutalismButton>EXECUTE_01</BrutalismButton>
          <BrutalismButton color="#fcee0a" textColor="#000">WARNING_LEVEL</BrutalismButton>
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Round & Playful</span>
        <div :class="$style.buttonRow">
          <BubbleButton color="#ff6b6b">Hey! Click me</BubbleButton>
          <BubbleButton color="#4ecdc4" icon="lucide:smile" />
          <BubbleButton variant="outline" color="#ff6b6b">Outline Soft</BubbleButton>
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Plastic</span>
        <div :class="$style.buttonRow">
          <ClayButton color="#84cc16">Confirm Action</ClayButton>
          <ClayButton color="#f97316" icon="lucide:flame">Hot Topic</ClayButton>
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Interface</span>
        <div :class="$style.buttonRow">
          <CyberButton color="#00f2fe" textColor="#000">INITIALIZE_SEQUENCE</CyberButton>
          <CyberButton color="#ff0055" textColor="#000" icon="lucide:zap">OVERLOAD</CyberButton>
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Crystal Layer</span>
        <div :class="$style.buttonRow">
          <GlassButton>Pure Crystal</GlassButton>
          <GlassButton icon="lucide:layers">Frost Blur</GlassButton>
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Extrusion</span>
        <div :class="$style.buttonRow">
          <NeuButton>Pressed State</NeuButton>
          <NeuButton color="#e0e5ec" textColor="#6d5dfc" icon="lucide:heart" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Racing HUD</span>
        <div :class="$style.buttonRow">
          <SpeedButton>FULL_THROTTLE</SpeedButton>
          <SpeedButton color="#fcee0a">NITRO_BOOST</SpeedButton>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoButton, 
  BrutalismButton, 
  BubbleButton, 
  ClayButton, 
  GlassButton, 
  CyberButton, 
  NeuButton, 
  SpeedButton 
} from "../components/buttons"
</script>

<style module>
/* ... Mantenemos tus estilos anteriores ... */
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f1f5f9;
  font-family: 'Inter', system-ui, sans-serif;
}

.header {
  max-width: 1000px;
  margin: 0 auto 80px auto;
  text-align: center;
}

.header h1 {
  font-size: 3rem;
  font-weight: 900;
  color: #0f172a;
  letter-spacing: -2px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(450px, 1fr));
  gap: 32px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: #ffffff;
  padding: 48px;
  border-radius: 24px;
  border: 1px solid rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.buttonRow {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  align-items: center;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
}

/* --- Fondos Contextuales --- */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }

/* Para que el Claymorphism resalte, un fondo pastel suave ayuda */
.clayBg { background: #f0f9ff; border-color: #e0f2fe; }

/* El Glassmorphism requiere un gradiente vibrante */
.glassBg {
  background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>