<script setup>
defineProps({
  message: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <aside class="toast-alert" role="status" aria-live="polite">
    {{ message }}
  </aside>
</template>
