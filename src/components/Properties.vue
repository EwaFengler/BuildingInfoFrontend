<template>
  <div class="properties">
    <div class="arrow" v-bind:class="expanded ? 'arrow-down' : 'arrow-right'"></div>
    <p v-on:click="expanded = !expanded">
      Properties
    </p>
    <ul class="building-info-list" v-if="expanded">
      <li>Area: {{ constProperties.area }}</li>
      <li>Cube: {{ constProperties.cube }}</li>
      <li>Light: {{ constProperties.light }}</li>
      <li>Heating: {{ constProperties.heating }}</li>
      <li>Heating per cube: {{ constProperties.heatingPerCube }}</li>
      <li>Light per area: {{ constProperties.lightPerArea }}</li>
      <li>Maintenance cost: {{ maintenanceCost }}</li>
    </ul>
  </div>
</template>

<script>
import {HTTP} from '../http-common'
import {bus} from '../main'

export default {
  name: 'Properties',
  props: {
    structureId: Number
  },
  data () {
    return {
      expanded: false,
      constProperties: {
        area: 0,
        cube: 0,
        light: 0,
        heating: 0,
        heatingPerCube: 0,
        lightPerArea: 0
      },
      maintenanceCost: 0
    }
  },
  created () {
    this.getProperties()
    bus.$on('structuresUpdated', () => {
      this.getProperties()
    })
    bus.$on('maintenanceCostUnitChanged', (cost) => {
      this.getMaintenanceCost(cost)
    })
  },
  methods: {
    getProperties: function () {
      for (let property in this.constProperties) {
        HTTP.get(`/structure/${property}/${this.structureId}`)
          .then(response => {
            this.constProperties[property] = response.data
          })
          .catch(e => {
            console.log(e)
          })
      }
    },
    getMaintenanceCost: function (cost) {
      HTTP.get(`/structure/maintenance-cost/${this.structureId}/${cost}`)
        .then(response => {
          this.maintenanceCost = response.data
        })
        .catch(e => {
          console.log(e)
        })
    }
  }
}
</script>
