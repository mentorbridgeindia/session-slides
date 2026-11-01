<script setup lang="ts">
interface Props {
  compact?: boolean
}
withDefaults(defineProps<Props>(), { compact: false })
</script>

<template>
  <div class="flex items-center gap-3" :class="compact ? '' : 'mb-6'">
    <img
      src="/mentorbridge-logo.jpg"
      alt="MentorBridge"
      :class="compact ? 'mb-logo' : 'mb-logo-lg'"
    />
  </div>
</template>
