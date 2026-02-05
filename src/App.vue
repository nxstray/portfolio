<script setup>
import { RouterView } from 'vue-router'
import { SpeedInsights } from "@vercel/speed-insights/vue"
</script>

<template>
  <RouterView />
  <SpeedInsights />
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body, #app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
}
</style>