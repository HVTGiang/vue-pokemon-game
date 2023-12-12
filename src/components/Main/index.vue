<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue'
import { cloneDeep } from 'lodash'

// components
import Welcome from 'src/components/Welcome/index.vue'
import Interact from 'src/components/Interact/index.vue'
import Result from 'src/components/Result/index.vue'
import Footing from 'src/components/Footer/index.vue'

// state of game
const GAME_STATE = {
  start: 'start',
  interact: 'interact',
  complete: 'complete'
}

const currentState = ref<string>(GAME_STATE.start)
const size = ref<number>(0)
const complete = ref<number>(0)
const timer = ref<number>(0)
const timerId = ref<number | undefined>()

const handleStart = (payload: number) => {
  reset()
  size.value = payload
  currentState.value = GAME_STATE.interact
}

const handleRestart = () => {
  currentState.value = GAME_STATE.start
}

const randomListPokemon = computed(() => {
  // initialize the list
  const totalList = Array.from({ length: 64 }, (_, index: number) => ({
    id: index + 1,
    url: `/images/${index + 1}.png`,
    isFlipped: false
  }))

  // create random list
  const chosenList = totalList
    .sort(() => 0.5 - Math.random())
    .slice(0, (size.value * size.value) / 2)

  // shuffle the list
  return [...cloneDeep(chosenList), ...cloneDeep(chosenList)]
    .sort(() => 0.5 - Math.random())
    .sort(() => 0.5 - Math.random())
})

const handleCompleteOne = () => {
  complete.value++
}

const reset = () => {
  size.value = 0
  complete.value = 0
  timer.value = 0
  timerId.value = undefined
}

watchEffect(() => {
  if (complete.value === randomListPokemon.value.length / 2 && size.value != 0) {
    currentState.value = GAME_STATE.complete
  }
})

watchEffect(() => {
  if (currentState.value === GAME_STATE.interact) {
    timerId.value = setInterval(() => {
      console.log(timer.value)
      timer.value++
    }, 1000)
  }
  if (currentState.value === 'complete') {
    clearInterval(timerId.value)
  }
})
</script>

<template>
  <Welcome v-if="currentState === GAME_STATE.start" @start="handleStart($event)" />
  <Interact
    v-if="currentState === GAME_STATE.interact"
    :size="size"
    @complete="handleCompleteOne"
  />
  <Result
    v-if="currentState === GAME_STATE.complete"
    :completeTime="timer"
    @restart="handleRestart"
  />
  <Footing />
</template>
