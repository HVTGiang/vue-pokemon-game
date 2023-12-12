<script setup lang="ts">
defineProps<{
  completeTime: number
}>()

defineEmits<{
  (eventName: 'restart'): void
}>()
</script>

<template>
  <div class="result-container">
    <h1 class="result__title">✨ Congratulations ✨</h1>
    <p class="result__time">{{ completeTime }} seconds</p>
    <div class="restart-button" @click="$emit('restart')">Play again</div>
    <div class="contact-block">
      <p class="contact__title">Contact me with</p>
      <div class="contact__logo-group">
        <a
          class="contact__logo-item"
          href="https://www.facebook.com/vutruonggiang.hoang.9/"
          target="_blank"
          rel="noopener"
        >
          <img
            src="https://img.icons8.com/color/48/000000/facebook.png"
            href="https://www.facebook.com/vutruonggiang.hoang.9/"
            target="_blank"
            rel="noopener"
          />
        </a>
        <a
          class="contact__logo-item"
          href="https://www.instagram.com/coconut2810/"
          target="_blank"
          rel="noopener"
        >
          <img src="https://img.icons8.com/fluent/48/000000/instagram-new.png" />
        </a>
        <a
          class="contact__logo-item"
          href="mailto:gianghvt.work@gmail.com"
          target="_blank"
          rel="noopener"
        >
          <img src="https://img.icons8.com/fluency/48/000000/gmail-new.png" />
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import './style.scss';
</style>
