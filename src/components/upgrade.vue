<script setup>
import { ref, watch, reactive, onMounted, inject } from 'vue'
import { UPGRADES } from './subcomps/UPGS.vue'

const test = ref(0)
let get = {}
let set = {}
let pointInt
let resetNum = ref(0)
const player = inject('player')

localStorage.setItem("test", 1)

function startValues() {
  upgBase()
  player.UPGS = UPGRADES
  player.points = 0
  player.bestPoints = 0
  player.pointMulti = 1
  player.basePointGain = 1
  player.pointGain = 1
  player.totalUpgs = 0
  player.boughtU22 = false
  player.class1 = ''
  player.activeTab = 1
}

function upgBase() {
  player.UPGS[1.11].cost = player.UPGS[1.11].baseCost
  player.UPGS[1.11].purchaseAmount = 0
  player.UPGS[1.21].cost = player.UPGS[1.21].baseCost
  player.UPGS[1.21].purchaseAmount = 0
  player.UPGS[1.22].purchased = false
}

function checkVals() {
  player.UPGS = set.UPGS
  player.points = set.points
  player.bestPoints = set.bestPoints
  player.pointMulti = set.pointMulti
  player.basePointGain = set.basePointGain
  player.pointGain = set.pointGain
  player.totalUpgs = set.totalUpgs
  player.boughtU22 = set.boughtU22
  player.class1 = set.class1
  player.activeTab = set.activeTab
}

function save() {
  localStorage.setItem("save-data", btoa(JSON.stringify(player)))
}

function load() {
  get = localStorage.getItem("save-data")
  if(get === null||get === undefined) player = startValues()
  else { 
    set = JSON.parse(atob(get))
    checkVals()
  }
}
onMounted(() => load())

function reset() {
  resetNum.value++
  localStorage.removeItem("save-data")
  load()
}

function incPoints() {
  player.pointGain = player.basePointGain * player.pointMulti
  player.points += player.pointGain
  player.bestPoints = Math.max(player.points, player.bestPoints)
}

function autoPoints() {
  player.pointGain = player.basePointGain * player.pointMulti
  player.points += player.pointGain / 8
  player.bestPoints = Math.max(player.points, player.bestPoints)
}

function buyUpg(id, single) {
  if(single) {
    if(player.points >= player.UPGS[id].cost && !player.UPGS[id].purchased) {
      player.points -= player.UPGS[id].cost
      player.UPGS[id].purchased = true
      player.totalUpgs += 1
    }
  } else {
    if(player.points >= player.UPGS[id].cost) {
      player.points -= player.UPGS[id].cost
      player.UPGS[id].cost *= player.UPGS[id].costScaling
      player.UPGS[id].purchased = true
      player.UPGS[id].purchaseAmount++
      player.totalUpgs += 1
    }
  }
  
}

watch(() => player.totalUpgs, () => {
  if(player.UPGS[1.11].purchased) {
    player.pointMulti = Math.pow(2, player.UPGS[1.11].purchaseAmount)
    if(player.UPGS[2.11].purchased) {
      player.pointMulti *= 1 + (player.totalUpgs / 20)
    }
  }
  if(player.UPGS[1.21].purchased) {
    player.basePointGain = player.UPGS[1.21].purchaseAmount + 1
  }
  if(player.UPGS[1.22].purchased) {
    player.boughtU22 = true
    pointInt = setInterval(autoPoints, 50)
  }
  
},
{ immediate: true }
) 

watch(resetNum, () => {
  clearInterval(pointInt)
})

</script>

<template>
  <p>Points: {{ player.points }}</p>
  <p v-if='player.bestPoints < 2000'>Reach 2000 points to unlock the skill tree</p>
  <br><br>
  <table style="width: 10%">
    <td style="width: 50%"><button @click='player.activeTab = 1'>Upgrade tree</button></td>
    <td style="width: 50%"><button v-if='player.bestPoints >= 2000' @click='player.activeTab = 2'>Skill tree</button></td>
  </table>
  <br>
  <button v-if="player.pointMulti == 1" @click="incPoints()">add 1 point</button>
  <button v-else @click="incPoints()">Add {{ player.pointMulti * player.basePointGain }} points</button>
  <br><br>
  <div v-if='player.activeTab == 1'>
    <button @click="buyUpg(1.11)">{{player.UPGS[1.11].desc}}<p>Cost: {{player.UPGS[1.11].cost}}</p></button>
    <br><br>
    <button @click="buyUpg(1.21)">{{player.UPGS[1.21].desc}}<p>Cost: {{player.UPGS[1.21].cost}}</p></button>
    <button @click="buyUpg(1.22, true)" :class='{purchased: player.UPGS[1.22].purchased}'>{{player.UPGS[1.22].desc}}<p>Cost: {{player.UPGS[1.22].cost}}</p></button><br><br><br>
  </div>

  <div v-if='player.activeTab == 2'>
  <button @click="buyUpg(2.11, true)" :class='{purchased: player.UPGS[2.11].purchased}'>{{player.UPGS[2.11].desc}}<p>Cost: {{player.UPGS[2.11].cost}}</p></button>
  </div>

  <button @click="save()">save</button>
  <button @click="load()">load</button><br>
  <button @click="reset()">reset</button>
  <button @click='player.points += 2000'>t</button>
</template>

<style scoped>

</style>
