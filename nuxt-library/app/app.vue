<template>
  <div class="screen-container">
    <div class="button-wrapper">
      <BubbleSelect />
    </div>
  </div>
</template>

<script setup lang="ts">
//@ts-ignore
import { BubbleSelect, CyberSelect, GlassSelect } from "./components/selects"
</script>

<style>
.screen-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background-color: #f3f4f6;
  margin: 0;
  padding: 0;
}

.button-wrapper {
  width: 100vw;
  height: 12vh;
}
</style>
