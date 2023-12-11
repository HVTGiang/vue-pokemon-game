<script setup lang="ts">
defineEmits<{
  (e: 'start', size: number): void
}>()
const levels = [
  {
    size: 4,
    label: 'Easy'
  },
  {
    size: 6,
    label: 'Normal'
  },
  {
    size: 8,
    label: 'Hard'
  },
  {
    size: 10,
    label: 'Unbelievable'
  }
]
</script>

<template>
  <div class="welcome__container">
    <h1 class="welcome__heading">✨ Pokemon Memories ✨</h1>
    <p class="welcome__description">Select mode to start game</p>
    <div class="levels">
      <div
        class="level__item"
        v-for="(level, index) in levels"
        :key="index"
        @click="$emit('start', level.size)"
      >
        <p class="item__number">{{ `${level.size}x${level.size}` }}</p>
        <p class="item__label">{{ level.label }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import './style.scss';
</style>
