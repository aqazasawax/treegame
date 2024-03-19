<script setup>
  import { inject } from 'vue'
  const player = inject('player')
</script>

<template>
  <br><br><br>
  <h2>Choose your class</h2>
  <br><br>
<table style="width: 100%">
  <tr>
    <th style="width: 33%">Warrior</th>
    <th style="width: 33%">Archer</th>
    <th style="width: 33%">Mage</th>
  </tr>
  <tr>
    <td><button @:click="player.class1 = 'warrior'">description 1</button></td>
    <td><button>Unavailable</button></td>
    <td><button>Unavailable</button></td>
  </tr>
</table>
</template>