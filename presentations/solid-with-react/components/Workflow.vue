<script setup lang="ts">
interface Step {
  label: string
  note?: string
}
interface Props {
  steps: Step[]
}
defineProps<Props>()
</script>

<template>
  <div class="mb-row">
    <template v-for="(step, i) in steps" :key="step.label">
      <div class="mb-step">
        <strong>{{ step.label }}</strong>
        <span v-if="step.note">{{ step.note }}</span>
      </div>
      <div v-if="i < steps.length - 1" class="mb-arrow">→</div>
    </template>
  </div>
</template>
