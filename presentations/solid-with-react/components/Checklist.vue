<script setup lang="ts">
interface Props {
  items: string[]
}
defineProps<Props>()
</script>

<template>
  <div>
    <div v-for="item in items" :key="item" class="mb-check">
      <div class="mb-box" />
      <span>{{ item }}</span>
    </div>
  </div>
</template>
