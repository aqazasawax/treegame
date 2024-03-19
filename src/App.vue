<script setup>
import { reactive, provide } from 'vue'
import upgrade from './components/upgrade.vue'
import { UPGRADES } from './components/subcomps/UPGS.vue'
import menus from './components/menu.vue'

const player = reactive({
  UPGS: UPGRADES,
  points: 0,
  bestPoints: 0,
  pointMulti: 1,
  basePointGain: 1,
  pointGain: 1,
  totalUpgs: 0,
  boughtU22: false,
  class1: '',
  activeTab: 1,
})
provide('player', player)
</script>

<template>
  <menus v-if="player.class1 == ''"></menus>
  <div :class='{invisible: player.class1 == ""}'>
    <upgrade></upgrade>
  </div>
</template>

<style scoped>

</style>
