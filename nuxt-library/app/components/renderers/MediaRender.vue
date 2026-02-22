<template>
  <video
    v-if="mediaType === 'video'"
    :src="url"
    v-bind="$attrs"
    autoplay
    muted
    loop
    playsinline
    @error="onMediaError"
  ></video>

  <NuxtImg
    v-else-if="mediaType === 'image' && !hasError"
    :src="url"
    :alt="alt"
    v-bind="$attrs"
    @error="onMediaError"
  />

  <img
    v-else
    :src="fallbackUrl"
    :alt="alt"
    v-bind="$attrs"
  />
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

const props = defineProps<{
  url: string;
  alt?: string;
  fallbackUrl?: string; // URL opcional para el fallback
}>();

const hasError = ref(false);

// Resetear el error si la URL cambia
watch(() => props.url, () => {
  hasError.value = false;
});

const mediaType = computed(() => {
  if (!props.url || hasError.value) return 'fallback';
  
  const videoExtensions = ['.mp4', '.webm', '.ogg', '.mov', '.m4v'];
  const isVideo = videoExtensions.some(ext => props.url.toLowerCase().endsWith(ext));
  
  return isVideo ? 'video' : 'image';
});

const onMediaError = () => {
  hasError.value = true;
};

// Puedes definir un fallback global aquí si no se pasa por props
const fallbackUrl = computed(() => props.fallbackUrl || '/images/placeholder.jpg');
</script>