<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { cloneDeep } from 'lodash'

// interfaces
import { IPokemon } from '../../core/interfaces/models/pokemon'

// components
import Card from '../Card/index.vue'

const props = defineProps<{
  size: number
}>()

const emit = defineEmits<{
  (e: 'complete'): void
}>()

const showPokemons = ref<(IPokemon & { isFlipped: boolean })[]>([])

// initialize the list
const totalList = computed(() => {
  return Array.from({ length: 64 }, (_, index: number) => ({
    id: index + 1,
    url: `/images/${index + 1}.png`,
    isFlipped: false
  }))
})

const randomListPokemon = () => {
  // create random list
  const chosenList = totalList.value
    .sort(() => 0.5 - Math.random())
    .slice(0, (props.size * props.size) / 2)

  // shuffle the list
  showPokemons.value = [...cloneDeep(chosenList), ...cloneDeep(chosenList)]
    .sort(() => 0.5 - Math.random())
    .sort(() => 0.5 - Math.random())
}

// 2 selected pokemon cards
const currentPair = ref<{ id: number; index: number }[]>([])

// all found pairs
const pairedList = ref<number[]>([])

const handleChoose = (chosenPokemon: IPokemon & { isFlipped: boolean }, index: number) => {

  showPokemons.value[index].isFlipped = true
  currentPair.value.push({ id: chosenPokemon.id, index })

  if (currentPair.value.length <= 1) {

    // first item in pair
    showPokemons.value[index].isFlipped = true

  } else {

    if (currentPair.value[0].id === currentPair.value[1].id) {

      // TRUE CASE
      if (currentPair.value[0].index === index) {

        // FAKE TRUE
        return

      } else {

        // REAL TRUE
        pairedList.value.push(currentPair.value[0].id)
        currentPair.value = []

        emit('complete')
      }
    } else {

      // FALSE CASE
      setTimeout(() => {
        showPokemons.value.forEach((showItem) => {

          if (currentPair.value.find((itemInPair) => itemInPair.id === showItem.id)) {
            showItem.isFlipped = false
          }
          
        })

        currentPair.value = []
        
      }, 900)
    }
  }
}

onMounted(() => {
  randomListPokemon()
})
</script>

<template>
  <div class="interact-container">
    <div class="card-list" :class="`card-list--${size}`">
      <Card
        v-for="(pokemon, index) in showPokemons"
        :key="index"
        :current-size="size"
        :pokemon="pokemon"
        :index="index"
        @choose="handleChoose"
        :is-paired="pairedList.includes(pokemon.id)"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
@import './style.scss';
</style>
