<template>
  <div v-bind="$attrs" :class="$style.rootContainer">
    <header :class="$style.doubleNavContainer">
      
      <div :class="$style.topBar" :style="topBarStyles">
        <div :class="$style.navWrapper">
          <div :class="$style.topBarContent">
            <slot name="topLeft">
              <span :class="$style.topText">{{ topText }}</span>
            </slot>
            <div :class="$style.topBarActions">
              <slot name="topRight" />
            </div>
          </div>
        </div>
      </div>

      <nav :class="$style.mainNav" :style="mainNavStyles">
        <div :class="$style.navWrapper">
          <div :class="$style.mainNavContent">
            
            <div :class="$style.navLogo">
              <NuxtLink to="/" :class="$style.logoLink">
                <slot name="logo">
                  <span :class="$style.logoText">BRAND</span>
                </slot>
              </NuxtLink>
            </div>

            <div :class="$style.navLinksContainer">
              <ul v-if="links.length" :class="$style.navLinks">
                <li v-for="link in links" :key="link.label">
                  <NuxtLink :to="link.to" :class="$style.linkItem">
                    {{ link.label }}
                  </NuxtLink>
                </li>
              </ul>
            </div>

            <div :class="$style.navActions">
              <slot name="actions" />
            </div>

          </div>
        </div>
      </nav>
    </header>
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
  topBg?: string;
  topColor?: string;
  topText?: string;
  mainBg?: string;
  mainColor?: string;
  height?: string;
  border?: string;
  shadow?: string | boolean;
}

const {
  links = [],
  topBg = "#1f2937",
  topColor = "#f3f4f6",
  topText = "Envío gratis en pedidos superiores a 50€",
  mainBg = "#ffffff",
  mainColor = "#111827",
  height = "70px",
  border = "1px solid rgba(0,0,0,0.1)",
  shadow = true
} = defineProps<Props>();

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)";

const topBarStyles = computed(() => ({
  "--top-bg": topBg,
  "--top-color": topColor,
}));

const mainNavStyles = computed(() => {
  let finalShadow = "none";
  
  if (typeof shadow === "string") {
    finalShadow = shadow;
  } else if (shadow === true) {
    finalShadow = DEFAULT_SHADOW;
  }

  return {
    "--main-bg": mainBg,
    "--main-color": mainColor,
    "--main-height": height,
    "--main-border": border,
    "--main-shadow": finalShadow,
  };
});
</script>

<style module>
.rootContainer {
  width: 100%;
}

.doubleNavContainer {
  width: 100%;
  z-index: 1000;
  position: sticky;
  top: 0;
  box-shadow: var(--main-shadow);
}

.navWrapper {
  width: 96%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 1.5rem;
}

.topBar {
  background-color: var(--top-bg);
  color: var(--top-color);
  font-size: 0.75rem;
  padding: 0.5rem 0;
}

.topBarContent {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.mainNav {
  background-color: var(--main-bg);
  color: var(--main-color);
  height: var(--main-height);
  display: flex;
  align-items: center;
  border-bottom: var(--main-border);
}

.mainNavContent {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.navLinksContainer {
  flex: 1;
  display: flex;
  justify-content: flex-end;
  padding-right: 2rem;
}

.navLinks {
  display: flex;
  gap: 2rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.linkItem {
  text-decoration: none;
  color: inherit;
  font-weight: 600;
  font-size: 0.9rem;
  transition: opacity 0.2s;
}

.linkItem:hover {
  opacity: 0.7;
}

.logoLink {
  text-decoration: none;
  color: inherit;
}

.logoText {
  font-size: 1.5rem;
  font-weight: 900;
  letter-spacing: -1px;
}
</style>