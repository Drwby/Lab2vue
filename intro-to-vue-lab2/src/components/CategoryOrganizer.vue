<template>
  <div class="categories-card">
    <div>{{ category }}</div>
    <div>{{ organizer }}</div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  category: string
  organizer: string
}>()
</script>

