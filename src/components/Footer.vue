<script setup lang="ts">
const { t, availableLocales, locale } = useI18n()

const toggleLocales = () => {
  // change to some real logic
  const locales = availableLocales
  locale.value = locales[(locales.indexOf(locale.value) + 1) % locales.length]
}
const routers = [{
  path: '/format_script',
  icon: 'script',
}]
</script>

<template>
  <nav text-xl mb-6>
    <RouterLink text-left class="icon-btn mx-2" to="/" :title="t('button.home')">
      <div i-carbon-campsite />
    </RouterLink>
    <button class="icon-btn mx-2 !outline-none" :title="t('button.toggle_dark')" @click="toggleDark()">
      <div i="carbon-sun dark:carbon-moon" />
    </button>
    <RouterLink class="icon-btn mx-2" to="/about" :title="t('button.about')">
      <div i-carbon-dicom-overlay />
    </RouterLink>
    <router-link v-for="{ path, icon } in routers" :key="path" :i="`carbon-${icon}`" class="icon-btn mx-2" :to="`${path}`">
      {{ path }}
    </router-link>
    <a class="icon-btn mx-2" rel="noreferrer" href="https://github.com/Jeffrey-mu/format-string" target="_blank" title="GitHub">
      <div i-carbon-logo-github />
    </a>
  </nav>
</template>
