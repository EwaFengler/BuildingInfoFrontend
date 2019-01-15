<template>
  <div id="app" class="wrapper">
    <button id="maintenance-cost-button">Set Maintenance cost</button>
    <div class="flex-container">
      <Card v-for="structure in structures"
            :key="structure.id"
            v-bind:structure="structure"
            v-bind:structureTypes="structureTypes"/>
    </div>
  </div>
</template>

<script>
import Card from './components/Card'
import {HTTP} from './http-common'
import {bus} from './main'

export default {
  name: 'App',
  components: {
    Card
  },
  data () {
    return {
      structureTypes: ['building', 'floor', 'room'],
      structures: null

    }
  },
  created () {
    this.getStructures()
    bus.$on('structuresUpdated', () => {
      this.getStructures()
    })
  },
  methods: {
    getStructures: function () {
      HTTP.get(`/structure/all`)
        .then(response => {
          this.structures = response.data
        })
        .catch(e => {
          console.log(e)
        })
    }
  }
}
</script>
