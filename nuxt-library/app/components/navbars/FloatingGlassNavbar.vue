<template>
  <div v-bind="$attrs" :class="$style.navWrapperRoot">
    <nav :class="$style.baseNavbar" :style="navbarStyles">
      <div :class="$style.navbarContainer">
        
        <div :class="$style.navbarLogo">
          <NuxtLink to="/" :class="$style.logoLink">
            <slot name="logo">
              <span :class="$style.defaultLogo">LOGO</span>
            </slot>
          </NuxtLink>
        </div>

        <div :class="$style.navbarLinksWrapper">
          <ul v-if="links.length" :class="$style.navbarLinks">
            <li v-for="link in links" :key="link.label">
              <NuxtLink :to="link.to" :class="$style.navLink">
                {{ link.label }}
              </NuxtLink>
            </li>
          </ul>
        </div>

        <div :class="$style.navbarActions">
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
  blur?: string;
  maxWidth?: string;
  border?: string;
  shadow?: string | boolean;
}

const {
  links = [],
  bg = 'rgba(255, 255, 255, 0.7)',
  textColor = '#1f2937',
  blur = '12px',
  maxWidth = '1200px',
  border = "1px solid rgba(0,0,0,0.1)",
  shadow = true
} = defineProps<Props>();

const DEFAULT_SHADOW = "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)";

const navbarStyles = computed(() => {
  let finalShadow = "none";
  
  if (typeof shadow === "string") {
    finalShadow = shadow;
  } else if (shadow === true) {
    finalShadow = DEFAULT_SHADOW;
  }

  return {
    '--nav-bg': bg,
    '--nav-text': textColor,
    '--nav-blur': blur,
    '--nav-max-width': maxWidth,
    "--n-border": border,
    "--n-shadow": finalShadow,
  };
});
</script>

<style module>
.navWrapperRoot {
  width: 100%;
}

.baseNavbar {
  width: 100%;
  background-color: var(--nav-bg);
  color: var(--nav-text);
  backdrop-filter: blur(var(--nav-blur));
  -webkit-backdrop-filter: blur(var(--nav-blur));
  box-shadow: var(--n-shadow);
  border-bottom: var(--n-border);
  transition: all 0.3s ease;
  z-index: 100;
}

.navbarContainer {
  max-width: var(--nav-max-width);
  margin: 0 auto;
  padding: 0.75rem 1.5rem;
  display: flex;
  align-items: center;
  gap: 2rem;
}

.navbarLinksWrapper {
  flex: 1;
  display: flex;
  justify-content: flex-end;
}

.navbarLinks {
  display: flex;
  gap: 2rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.navLink {
  text-decoration: none;
  color: inherit;
  font-weight: 500;
  opacity: 0.8;
  transition: opacity 0.2s;
  white-space: nowrap;
}

.navLink:hover {
  opacity: 1;
}

.logoLink {
  text-decoration: none;
  color: inherit;
}

.defaultLogo {
  font-weight: 800;
  font-size: 1.25rem;
  letter-spacing: -0.05em;
}

.navbarActions {
  display: flex;
  align-items: center;
  gap: 1rem;
}
</style>