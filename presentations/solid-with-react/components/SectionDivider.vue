<script setup lang="ts">
interface Props {
  number: string
  title: string
  subtitle?: string
}
defineProps<Props>()
</script>

<template>
  <div class="h-full flex flex-col justify-center gap-3">
    <BrandLogo />
    <div class="mb-section-num">{{ number }}</div>
    <div>
      <h1 class="!m-0 !text-3xl">{{ title }}</h1>
      <div class="mb-rule mt-3 mb-3" />
      <p v-if="subtitle" class="mb-lead !m-0">{{ subtitle }}</p>
    </div>
  </div>
</template>
