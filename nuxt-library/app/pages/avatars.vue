<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>IDENTITY_NODES</h1>
      <p>AVATAR_COLLECTION // USER_REPRESENTATION</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Frames</span>
        <div :class="$style.avatarGroup">
          <BentoAvatar size="sm" status="online" src="https://i.pravatar.cc/150?u=1" />
          <BentoAvatar size="md" status="busy" src="https://i.pravatar.cc/150?u=2" />
          <BentoAvatar size="lg" initials="JD" color="#e0e7ff" />
          <BentoAvatar size="xl" src="https://i.pravatar.cc/150?u=3" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Pop Border</span>
        <div :class="$style.avatarGroup">
          <BrutalismAvatar size="sm" src="https://i.pravatar.cc/150?u=4" />
          <BrutalismAvatar size="md" color="#fcee0a" initials="BR" />
          <BrutalismAvatar size="lg" src="https://i.pravatar.cc/150?u=5" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Glitch Profile</span>
        <div :class="$style.avatarGroup">
          <CyberAvatar size="md" status="online" src="https://i.pravatar.cc/150?u=6" />
          <CyberAvatar size="lg" status="busy" src="https://i.pravatar.cc/150?u=7" />
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Crystal Identity</span>
        <div :class="$style.avatarGroup">
          <GlassAvatar size="md" src="https://i.pravatar.cc/150?u=8" />
          <GlassAvatar size="lg" color="rgba(255,255,255,0.2)" initials="GL" />
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Surface</span>
        <div :class="$style.avatarGroup">
          <NeuAvatar size="md" src="https://i.pravatar.cc/150?u=9" />
          <NeuAvatar size="lg" initials="NM" />
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Squishy</span>
        <div :class="$style.avatarGroup">
          <ClayAvatar size="md" color="#ffedd5" src="https://i.pravatar.cc/150?u=10" />
          <ClayAvatar size="lg" color="#fce7f3" initials="CL" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // HUD Pilot</span>
        <div :class="$style.avatarGroup">
          <SpeedAvatar size="lg" status="online" src="https://i.pravatar.cc/150?u=11" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Liquid Form</span>
        <div :class="$style.avatarGroup">
          <BubbleAvatar size="md" src="https://i.pravatar.cc/150?u=12" />
          <BubbleAvatar size="lg" src="https://i.pravatar.cc/150?u=13" />
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import {
  BubbleAvatar,
  CyberAvatar,
  GlassAvatar,
  BentoAvatar,
  BrutalismAvatar,
  ClayAvatar,
  NeuAvatar,
  SpeedAvatar,
} from "../components/avatars";
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 20px;
  background-color: #f8fafc;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 900;
  color: #0f172a;
  letter-spacing: -1.5px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 24px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 32px;
  border-radius: 28px;
  border: 1px solid rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1px;
}

.avatarGroup {
  display: flex;
  align-items: flex-end;
  gap: 16px;
  flex-wrap: wrap;
}

/* Fondos contextuales */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #fff7ed; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>