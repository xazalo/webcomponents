<template>
  <div v-bind="$attrs" :class="$style.glassAvatar" :style="glassStyles">
    <div :class="$style.inner">
      <NuxtImg
        v-if="src"
        :src="src"
        :alt="alt"
        :class="$style.avatarImage"
        provider="ipx"
        loading="lazy"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
defineOptions({ inheritAttrs: false });

const props = defineProps<{
  src: string;
  alt?: string;
  shadow?: string | boolean;
}>();

const glassStyles = computed(() => ({
  "--g-shadow":
    props.shadow === true
      ? "0 10px 15px -3px rgba(0, 0, 0, 0.2)"
      : props.shadow || "none",
}));
</script>

<style module>
.glassAvatar {
  width: 50px;
  height: 50px;
  padding: 3px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border-radius: 50%;
  box-shadow: var(--g-shadow);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.inner {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  overflow: hidden;
  background: #eee;
}

.inner img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
