<template>
  <div id="app" class="wrapper">
    <button id="maintenance-cost-button">Set Maintenance cost</button>
    <div class="flex-container">
      <Card v-for="structure in structures"
        :key="structure.id"
        v-bind:structure="structure"
        v-bind:structureTypes="structureTypes"/>
      <AddCard v-bind:structureType="structureTypes[0]"/>
  </div>
  </div>
</template>

<script>
import Card from './components/Card'
import AddCard from './components/AddCard'
import {HTTP} from './http-common'

export default {
  name: 'App',
  components: {
    Card,
    AddCard
  },
  data () {
    return {
      structureTypes: ['building', 'floor', 'room'],
      structures: null

    }
  },
  created () {
    HTTP.get(`/structure/all`)
      .then(response => {
        this.structures = response.data
      })
      .catch(e => {
        console.log(e)
      })
  }
}
</script>
