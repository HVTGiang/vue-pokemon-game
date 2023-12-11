<script setup lang="ts">

import { IPokemon } from '../../core/interfaces/models/pokemon'

const props = defineProps<{
  currentSize: number
  pokemon: IPokemon & { isFlipped: boolean }
  index: number
  isPaired: boolean
}>()

const emit = defineEmits<{
  (e: 'choose', chosenPokemon: IPokemon & { isFlipped: boolean }, index: number): void
}>()

const handleClick = () => {
  if (props.isPaired || props.pokemon.isFlipped) return

  // isFlipping.value = true
  emit('choose', props.pokemon, props.index)
}
</script>

<template>
  <div
    class="card-container"
    :class="[`card-${currentSize}`, isPaired && 'is-paired']"
    @click="handleClick"
  >
    <div class="card-inner" :class="{ 'is-chosen': pokemon.isFlipped }">
      <div class="card-head card-face">
        <img :src="pokemon.url" />
      </div>
      <div class="card-back card-face"></div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import './style.scss';
</style>
