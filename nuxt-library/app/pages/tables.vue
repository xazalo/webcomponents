<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>DATATABLES_MULTIVERSE</h1>
      <p>ESTRUCTURAL_DATA // MULTI-STYLE_INTERFACES</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Soft</span>
        <BentoTable :items="tableData" />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // High Contrast</span>
        <BrutalismTable :items="tableData" color="#fcee0a" />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Interface</span>
        <CyberTable :items="tableData" />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Crystal Layer</span>
        <GlassTable :items="tableData" />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Extrusion</span>
        <NeuTable :items="tableData" />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Plastic</span>
        <ClayTable :items="tableData" />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Racing HUD</span>
        <SpeedTable :items="tableData" />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Round & Playful</span>
        <BubbleTable :items="tableData" color="#ff6b6b" />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoTable, 
  BrutalismTable, 
  BubbleTable, 
  ClayTable, 
  CyberTable, 
  GlassTable, 
  NeuTable, 
  SpeedTable 
} from "../components/tables"

const tableData = [
  { id: '01', user: 'Alpha_Unit', status: 'ACTIVE', signal: '98%' },
  { id: '02', user: 'Beta_Core', status: 'IDLE', signal: '45%' },
  { id: '03', user: 'Ghost_Node', status: 'OFFLINE', signal: '00%' },
];
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f1f5f9;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 80px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 900;
  color: #0f172a;
  letter-spacing: -1px;
}

.header p {
  font-family: monospace;
  color: #64748b;
  letter-spacing: 2px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
  gap: 32px;
  max-width: 1600px;
  margin: 0 auto;
}

.card {
  background: #ffffff;
  padding: 32px;
  border-radius: 24px;
  border: 1px solid rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  gap: 24px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02);
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: #94a3b8;
}

/* Contextos Especiales para que las tablas resalten */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0fdf4; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #4f46e5 0%, #9333ea 100%); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
  .card { padding: 20px; }
}
</style>