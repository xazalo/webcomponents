<template>
  <div v-bind="$attrs" :class="$style.bubbleAvatar" :style="bubbleStyles">
    <div :class="$style.imageWrapper">
      <NuxtImg
        v-if="src"
        :src="src"
        :alt="alt"
        :class="$style.avatarImage"
        provider="ipx"
        loading="lazy"
      />
      <span v-else>{{ initials }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  src?: string;
  initials?: string;
  alt?: string;
  color?: string;
  shadow?: string | boolean;
}>();

const bubbleStyles = computed(() => ({
  "--b-color": props.color || "#4f46e5",
  "--b-shadow":
    props.shadow === true
      ? "0 8px 20px -5px rgba(0,0,0,0.3)"
      : props.shadow || "none",
}));
</script>

<style module>
.bubbleAvatar {
  width: 60px;
  height: 60px;
  display: inline-block;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.imageWrapper {
  width: 100%;
  height: 100%;
  background: var(--b-color);
  box-shadow: var(--b-shadow);
  border-radius: 60% 40% 70% 30% / 40% 50% 60% 40%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  color: white;
  font-weight: 800;
  transition: all 0.5s ease;
}

.imageWrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.bubbleAvatar:hover .imageWrapper {
  border-radius: 40% 60% 30% 70% / 60% 40% 70% 30%;
  transform: rotate(5deg) scale(1.1);
}
</style>
