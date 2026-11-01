<script setup lang="ts">
interface Props {
  leftTitle: string
  rightTitle: string
  leftItems: string[]
  rightItems: string[]
}
defineProps<Props>()
</script>

<template>
  <div class="mb-two">
    <div class="mb-card">
      <div class="mb-kicker">{{ leftTitle }}</div>
      <ul class="mb-list">
        <li v-for="item in leftItems" :key="item">{{ item }}</li>
      </ul>
    </div>
    <div class="mb-card-pink">
      <div class="mb-kicker">{{ rightTitle }}</div>
      <ul class="mb-list">
        <li v-for="item in rightItems" :key="item">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>
