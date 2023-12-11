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
    :class="[`card--${currentSize}`, isPaired && 'card--is-paired']"
    @click="handleClick"
  >
    <div class="card__inner" :class="{ 'card--is-chosen': pokemon.isFlipped }">
      <div class="card__face--head card__face">
        <img :src="pokemon.url" alt="Pokemon image" />
      </div>
      <div class="card__face--back card__face"></div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import './style.scss';
</style>
