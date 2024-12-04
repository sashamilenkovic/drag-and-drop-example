<script setup lang="ts">
const props = defineProps<{
  todo: string;
}>();
</script>

<template>
  <span>
    {{ todo }}
  </span>
</template>
