<script lang="ts" setup>
defineProps<{ icon?: string; name: string; active?: boolean }>();
</script>

<template>
  <button
    class="interactive-network-tile"
    :class="[`${active ? 'border-primary/50 border-2' : ''}`]"
    :disabled="active"
  >
    <img v-if="icon" :src="icon" class="h-8 w-8 rounded-full border" />
    <h1 class="font-semibold">{{ name }}</h1>
  </button>
</template>

<style lang="scss" scoped>
.interactive-network-tile {
  @apply flex items-center;
  @apply w-full px-4 py-3;
  @apply border rounded-md space-x-4 bg-purple/5 transition-all;
  @apply enabled:hover:scale-98 hover:border-primary/50 enabled:active:scale-95;
}
</style>
