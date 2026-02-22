<template>
  <div v-bind="$attrs" :class="$style.avatarRoot" :style="avatarStyles">
    <div :class="$style.cyberFrame">
      <NuxtImg
        v-if="src"
        :src="src"
        :alt="alt"
        :class="$style.avatarImage"
        provider="ipx"
        loading="lazy"
      />
      <div v-else :class="$style.initials">{{ initials }}</div>
    </div>
    <div v-if="status" :class="[$style.statusBadge, $style[status]]"></div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
defineOptions({ inheritAttrs: false });

interface Props {
  src?: string;
  alt?: string;
  initials?: string;
  size?: "sm" | "md" | "lg";
  color?: string;
  status?: "online" | "offline" | "busy";
  shadow?: string | boolean;
}

const props = withDefaults(defineProps<Props>(), {
  size: "md",
  color: "#4f46e5",
  shadow: true,
  initials: "??",
});

const avatarStyles = computed(() => {
  const sizes = { sm: "32px", md: "48px", lg: "64px" };
  const DEFAULT_SHADOW = "0 4px 10px rgba(0,0,0,0.2)";

  return {
    "--a-size": sizes[props.size],
    "--a-color": props.color,
    "--a-shadow":
      typeof props.shadow === "string"
        ? props.shadow
        : props.shadow
          ? DEFAULT_SHADOW
          : "none",
  };
});
</script>

<style module>
.avatarRoot {
  position: relative;
  width: var(--a-size);
  height: var(--a-size);
  display: inline-block;
}

.cyberFrame {
  width: 100%;
  height: 100%;
  background: var(--a-color);
  clip-path: polygon(20% 0%, 100% 0%, 100% 80%, 80% 100%, 0% 100%, 0% 20%);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: var(--a-shadow);
}

.avatarImage {
  width: 90%;
  height: 90%;
  object-fit: cover;
  clip-path: polygon(20% 0%, 100% 0%, 100% 80%, 80% 100%, 0% 100%, 0% 20%);
}

.initials {
  color: white;
  font-weight: bold;
  font-size: calc(var(--a-size) * 0.4);
}

.statusBadge {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 25%;
  height: 25%;
  border-radius: 50%;
  border: 2px solid white;
}

.online {
  background: #10b981;
}
.offline {
  background: #9ca3af;
}
.busy {
  background: #ef4444;
}
</style>
