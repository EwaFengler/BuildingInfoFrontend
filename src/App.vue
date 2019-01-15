<template>
  <div id="app" class="wrapper">
    <Options/>
    <div class="flex-container">
      <Card v-for="structure in structures"
            :key="structure.id"
            v-bind:structure="structure"
            v-bind:structureTypes="structureTypes"/>
    </div>
  </div>
</template>

<script>
import Options from './components/Options'
import Card from './components/Card'
import {HTTP} from './http-common'
import {bus} from './main'

export default {
  name: 'App',
  components: {
    Options,
    Card
  },
  data () {
    return {
      structureTypes: ['building', 'floor', 'room'],
      heatingPerCubeThreshold: 0,
      structures: null
    }
  },
  created () {
    this.getStructures()
    bus.$on('structuresUpdated', () => {
      this.getStructures()
    })
    bus.$on('heatingThresholdChanged', (threshold) => {
      this.heatingPerCubeThreshold = threshold
      this.getStructures()
    })
  },
  methods: {
    getStructures: function () {
      HTTP.get(`/structure/heatingPerCubeGreaterThan/${this.heatingPerCubeThreshold}`)
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
