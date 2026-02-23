<template>
  <div :class="$style.container">
    <header :class="$style.header">
      <h1 :class="$style.title">STAR_RATING_SYSTEM</h1>
      <p :class="$style.subtitle">ATOMIC_COMPONENT // HALF_STAR_SUPPORT // FULLY_CONFIGURABLE</p>
    </header>

    <div :class="$style.showcase">
      
      <section :class="$style.card">
        <span :class="$style.tag">Bento Default</span>
        <div :class="$style.demo">
          <StarRating :model-value="4.5" show-value />
          <p :class="$style.label">Standard Feedback</p>
        </div>
      </section>

      <section :class="[$style.card, $style.cyber]">
        <span :class="$style.tag">Cyber / Neon</span>
        <div :class="$style.demo">
          <StarRating 
            :model-value="4.8" 
            active-color="#00f2fe" 
            inactive-color="#1a1a1a"
            size="1.5rem" 
            show-value 
          />
          <p :class="$style.label">Neural link quality: 98%</p>
        </div>
      </section>

      <section :class="[$style.card, $style.brutal]">
        <span :class="$style.tag">Brutalism</span>
        <div :class="$style.demo">
          <StarRating 
            :model-value="3" 
            active-color="#000" 
            inactive-color="transparent"
            size="2rem" 
            gap="2px"
          />
          <p :class="$style.label">RAW_SCORE: 03</p>
        </div>
      </section>

      <section :class="[$style.card, $style.glass]">
        <span :class="$style.tag">Glass / Luxury</span>
        <div :class="$style.demo">
          <StarRating 
            :model-value="5" 
            active-color="#fff" 
            inactive-color="rgba(255,255,255,0.2)"
            size="1.2rem" 
          />
          <p :class="$style.label">Premium Service</p>
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Scale Variants</span>
        <div :class="$style.verticalStack">
          <StarRating :model-value="4.2" size="0.8rem" show-value />
          <StarRating :model-value="4.2" size="1.2rem" show-value />
          <StarRating :model-value="4.2" size="2rem" show-value />
        </div>
      </section>

    </div>
  </div>
</template>

<script setup lang="ts">
import { StarRating } from '../components/ratings';
</script>

<style module>
.container {
  min-height: 100vh;
  padding: 80px 20px;
  background: #f8fafc;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.title {
  font-size: 2.5rem;
  font-weight: 900;
  letter-spacing: -1px;
  margin: 0;
}

.subtitle {
  color: #64748b;
  font-weight: 600;
  font-size: 0.8rem;
  margin-top: 10px;
}

.showcase {
  max-width: 1000px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}

.card {
  background: white;
  padding: 32px;
  border-radius: 24px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 20px;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1px;
}

.label {
  margin-top: 12px;
  font-size: 0.9rem;
  font-weight: 600;
  color: #475569;
}

.verticalStack {
  display: flex;
  flex-direction: column;
  gap: 16px;
  align-items: center;
}

/* --- VARIACIONES DE CONTEXTO --- */

.cyber {
  background: #0a0a0b;
  border-color: #00f2fe44;
}
.cyber .label { color: #00f2fe; }

.brutal {
  background: #fcee0a;
  border: 4px solid #000;
  border-radius: 0;
  box-shadow: 8px 8px 0 #000;
}
.brutal .label { color: #000; text-transform: uppercase; font-weight: 900; }

.glass {
  background: linear-gradient(135deg, #6366f1, #a855f7);
  border: none;
}
.glass .label { color: white; opacity: 0.9; }

@media (max-width: 640px) {
  .showcase { grid-template-columns: 1fr; }
}
</style>