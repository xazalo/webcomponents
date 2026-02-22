<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>INPUT_LABORATORY</h1>
      <p>DATA_ENTRY // VISUAL_LANGUAGES</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Standard</span>
        <div :class="$style.stack">
          <BentoInput v-model="form.bento1" placeholder="User Name" color="#4f46e5" />
          <BentoInput v-model="form.bento2" placeholder="Email Address" color="#4f46e5" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // RAW_TEXT</span>
        <div :class="$style.stack">
          <BrutalismInput v-model="form.brutal1" placeholder="ENTER_COMMAND" color="#A3E635" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Input</span>
        <div :class="$style.stack">
          <CyberInput v-model="form.cyber1" placeholder="ACCESS_KEY..." color="#00f3ff" />
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Clear Vision</span>
        <div :class="$style.stack">
          <GlassInput v-model="form.glass1" placeholder="Floating text..." />
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Surface</span>
        <div :class="$style.stack">
          <NeuInput v-model="form.neu1" placeholder="Inscribed..." />
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // Soft Volume</span>
        <div :class="$style.stack">
          <ClayInput v-model="form.clay1" placeholder="Type here..." color="#f472b6" />
        </div>
      </section>

      <section :class="[$style.card, $style.speedBg]">
        <span :class="$style.tag">Speed // Kinetic Motion</span>
        <div :class="$style.stack">
          <SpeedInput v-model="form.speed1" placeholder="QUICK_INPUT" color="#ff4d00" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Elastic Form</span>
        <div :class="$style.stack">
          <BubbleInput v-model="form.bubble1" placeholder="Search..." color="#3b82f6" />
        </div>
      </section>
    </div>

    <footer :class="$style.previewFooter">
      <div :class="$style.debugBox">
        <code>{{ form }}</code>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  BubbleInput, 
  CyberInput, 
  GlassInput, 
  BentoInput, 
  BrutalismInput, 
  ClayInput, 
  NeuInput, 
  SpeedInput 
} from "../components/inputs";

const form = ref({
  bento1: '', bento2: '',
  brutal1: '',
  cyber1: '',
  glass1: '',
  neu1: '',
  clay1: '',
  speed1: '',
  bubble1: ''
});
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f8fafc;
  font-family: 'Inter', system-ui, sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 3rem;
  font-weight: 900;
  color: #020617;
  letter-spacing: -2px;
  margin-bottom: 8px;
}

.header p {
  color: #64748b;
  font-weight: 600;
  letter-spacing: 2px;
  font-size: 0.8rem;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
  gap: 32px;
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
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02);
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1.5px;
  border-left: 3px solid currentColor;
  padding-left: 10px;
}

.stack {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
}

/* --- Contextos de Color --- */
.darkCard { background: #0a0a0b; }
.darkCard .tag { color: #475569; }

.neuBg { background: #e0e5ec; }
.neuBg .tag { color: #748194; }

.clayBg { background: #fdf2f8; }

.glassBg { 
  background: url('https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?auto=format&fit=crop&q=80&w=800') center;
  background-size: cover;
  position: relative;
}

.speedBg {
  background: #111;
  background-image: radial-gradient(circle at 2px 2px, #333 1px, transparent 0);
  background-size: 20px 20px;
}

/* --- Footer Debug --- */
.previewFooter {
  margin-top: 80px;
  display: flex;
  justify-content: center;
}

.debugBox {
  background: #1e293b;
  padding: 20px;
  border-radius: 16px;
  color: #38bdf8;
  font-size: 0.8rem;
  max-width: 100%;
  overflow-x: auto;
}

@media (max-width: 768px) {
  .grid { grid-template-columns: 1fr; }
  .showroomContainer { padding: 40px 20px; }
}
</style>