<template>
  <div class="properties">
    <div class="arrow" v-bind:class="expanded ? 'arrow-down' : 'arrow-right'"></div>
    <p v-on:click="expanded = !expanded">
      Properties
    </p>
    <ul class="properties-list" v-if="expanded">
      <li>Area: {{ constProperties.area }}</li>
      <li>Cube: {{ constProperties.cube }}</li>
      <li>Light: {{ constProperties.light }}</li>
      <li>Heating: {{ constProperties.heating }}</li>
      <li>Heating per cube: {{ constProperties.heatingPerCube }}</li>
      <li>Light per area: {{ constProperties.lightPerArea }}</li>
      <li>Maintenance cost: {{ calcProperties.maintenanceCost }}</li>
    </ul>
  </div>
</template>

<script>
import {HTTP} from '../http-common'

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
      calcProperties: {
        maintenanceCost: 1000
      }
    }
  },
  created () {
    for (let property in this.constProperties) {
      HTTP.get(`/structure/${property}/${this.structureId}`)
        .then(response => {
          this.constProperties[property] = response.data
        })
        .catch(e => {
          // console.log(e)
        })
    }
  }
}
</script>
