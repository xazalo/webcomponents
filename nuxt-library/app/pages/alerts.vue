<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>UI_ALERTS_COLLECTION</h1>
      <p>Explorando los multiversos de diseño // v1.0</p>
    </header>

    <div :class="$style.alertGrid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento Grid</span>
        <BentoAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism</span>
        <BrutalismAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble / Soft</span>
        <BubbleAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close" 
        />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism / 3D Soft</span>
        <ClayAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close" 
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk</span>
        <CyberAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close" 
        />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism</span>
        <GlassAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close" 
        />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism</span>
        <NeuAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close" 
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed Style</span>
        <SpeedAlert 
          title="System Update" 
          message="New features have been deployed to your workspace."
          type="info"
          actionText="done"
          buttonText="close"
        />
      </section>
    </div>
  </div>
</template>

<script setup>
import { 
  BentoAlert, 
  BrutalismAlert, 
  BubbleAlert,
  ClayAlert, 
  CyberAlert, 
  GlassAlert, 
  NeuAlert, 
  SpeedAlert 
} from "../components/alerts"
</script>

<style module>
/* ... mantenemos tus estilos anteriores ... */
.showroomContainer {
  min-height: 100vh;
  padding: 40px 20px;
  background-color: #f0f2f5;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 50px;
}

.alertGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 30px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding: 24px;
  border-radius: 12px;
  background: white;
  border: 1px solid #e0e0e0;
}

.tag {
  font-size: 0.7rem;
  font-weight: bold;
  text-transform: uppercase;
  color: #999;
  letter-spacing: 1px;
}

/* --- Contextos Visuales --- */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0f7ff; border-color: #e0e7ff; }

.glassBg {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  position: relative;
  border: none;
}

@media (max-width: 500px) {
  .alertGrid { grid-template-columns: 1fr; }
}
</style>