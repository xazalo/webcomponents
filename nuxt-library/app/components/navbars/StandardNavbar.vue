<template>
  <div v-bind="$attrs" :class="$style.navContainer">
    <nav :class="$style.standardNavbar" :style="navStyles">
      <div :class="$style.navWrapper">
        <div :class="$style.navSection">
          <NuxtLink to="/" :class="$style.logoLink">
            <slot name="logo">
              <span :class="$style.logoText">BRAND</span>
            </slot>
          </NuxtLink>
        </div>

        <div :class="$style.navCentralGroup">
          <ul :class="$style.navLinks" v-if="links.length">
            <li v-for="link in links" :key="link.label">
              <NuxtLink :to="link.to">{{ link.label }}</NuxtLink>
            </li>
          </ul>
        </div>

        <div v-if="$slots.actions" :class="$style.navSection">
          <slot name="actions" />
        </div>
      </div>
    </nav>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

defineOptions({
  inheritAttrs: false
});

interface NavLink {
  label: string;
  to: string;
}

interface Props {
  links?: NavLink[];
  bg?: string;
  textColor?: string;
  height?: string;
  border?: string;
  shadow?: string | boolean; // Ahora acepta un string de sombra o un booleano
}

const {
  links = [],
  bg = "#ffffff",
  textColor = "#111827",
  height = "70px",
  border = "1px solid rgba(0,0,0,0.1)",
  shadow = true
} = defineProps<Props>();

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)";

const navStyles = computed(() => {
  let finalShadow = "none";
  
  if (typeof shadow === "string") {
    finalShadow = shadow;
  } else if (shadow === true) {
    finalShadow = DEFAULT_SHADOW;
  }

  return {
    "--n-bg": bg,
    "--n-text": textColor,
    "--n-height": height,
    "--n-border": border,
    "--n-shadow": finalShadow,
  };
});
</script>

<style module>
.navContainer {
  width: 100%;
}

.standardNavbar {
  position: sticky;
  top: 0;
  z-index: 1000;
  width: 100%;
  height: var(--n-height);
  background-color: var(--n-bg);
  color: var(--n-text);
  border-bottom: var(--n-border);
  box-shadow: var(--n-shadow);
  display: flex;
  align-items: center;
  transition: all 0.2s ease;
}

.navWrapper {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navSection {
  display: flex;
  align-items: center;
}

.navCentralGroup {
  flex: 1;
  display: flex;
  justify-content: flex-end;
}

.navLinks {
  display: flex;
  gap: 2.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.navLinks a {
  text-decoration: none;
  color: inherit;
  font-size: 0.95rem;
  font-weight: 500;
}

.logoLink {
  text-decoration: none;
  color: inherit;
}

.logoText {
  font-size: 1.5rem;
  font-weight: 900;
}
</style>