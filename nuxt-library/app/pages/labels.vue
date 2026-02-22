<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>LABEL_IDENTITIES</h1>
      <p>METADATA_VISUALIZER // TAG_TAXONOMY</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.cardTag">Bento // Semantic Tags</span>
        <div :class="$style.labelCloud">
          <BentoLabel color="#4f46e5">UPDATE_AVAILABLE</BentoLabel>
          <BentoLabel color="#10b981">SYSTEM_STABLE</BentoLabel>
          <BentoLabel color="#f59e0b">PENDING_REVIEW</BentoLabel>
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.cardTag">Brutalism // Hard Badges</span>
        <div :class="$style.labelCloud">
          <BrutalismLabel color="#FF3E00">CRITICAL_ERROR</BrutalismLabel>
          <BrutalismLabel color="#A3E635">LIVE_DEPLOY</BrutalismLabel>
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.cardTag">Cyber // Encryption Level</span>
        <div :class="$style.labelCloud">
          <CyberLabel color="#00f3ff">ENCRYPTED_AES</CyberLabel>
          <CyberLabel color="#ff0055">SECURITY_BREACH</CyberLabel>
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.cardTag">Glass // Frosted Meta</span>
        <div :class="$style.labelCloud">
          <GlassLabel>v2.4.0-alpha</GlassLabel>
          <GlassLabel>Shared_Folder</GlassLabel>
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.cardTag">Neu // Inset State</span>
        <div :class="$style.labelCloud">
          <NeuLabel>HAPTIC_ID</NeuLabel>
          <NeuLabel>IDLE_STATE</NeuLabel>
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.cardTag">Clay // Squishy Tags</span>
        <div :class="$style.labelCloud">
          <ClayLabel color="#f472b6">PLAYFUL</ClayLabel>
          <ClayLabel color="#3b82f6">SOFT_UI</ClayLabel>
        </div>
      </section>

      <section :class="[$style.card, $style.speedBg]">
        <span :class="$style.cardTag">Speed // Fast Track</span>
        <div :class="$style.labelCloud">
          <SpeedLabel color="#ff4d00">TOP_SPEED</SpeedLabel>
          <SpeedLabel color="#fff">RPM_LIMIT</SpeedLabel>
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.cardTag">Bubble // Floating Info</span>
        <div :class="$style.labelCloud">
          <BubbleLabel color="#6366f1">In_Orbit</BubbleLabel>
          <BubbleLabel color="#ec4899">Pop_Status</BubbleLabel>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoLabel, 
  BrutalismLabel, 
  CyberLabel, 
  GlassLabel, 
  NeuLabel, 
  ClayLabel, 
  SpeedLabel, 
  BubbleLabel 
} from "../components/labels";
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f8fafc;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 950;
  color: #0f172a;
  letter-spacing: -1px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  gap: 24px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 40px;
  border-radius: 32px;
  border: 1px solid rgba(0,0,0,0.03);
  display: flex;
  flex-direction: column;
  gap: 24px;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
}

.cardTag {
  font-size: 0.65rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 2px;
}

.labelCloud {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  align-items: center;
}

/* --- Contextos Visuales --- */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #eff6ff; border: none; }

.glassBg { 
  background: linear-gradient(225deg, #1e293b 0%, #0f172a 100%);
  background-image: url("https://www.transparenttextures.com/patterns/cubes.png");
}

.speedBg {
  background: #111;
  border-left: 8px solid #ff4d00;
}

@media (max-width: 768px) {
  .grid { grid-template-columns: 1fr; }
}
</style>