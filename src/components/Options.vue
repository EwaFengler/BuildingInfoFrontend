<template>
  <div id="options">
    <p>Options</p>
    <ul class="building-info-list">
      <li>Heating Per Cube Threshold: <input type="text" v-model="threshold" v-on:keyup="thresholdChange"/></li>
      <li>Maintenance Cost Unit:  <input type="text" v-model="unit" v-on:keyup="unitChange"/></li>
    </ul>
  </div>
</template>

<script>
import {bus} from '../main'

export default {
  name: 'Options',
  data () {
    return {
      threshold: 0,
      unit: 0
    }
  },
  methods: {
    thresholdChange: function () {
      if (this.threshold && !isNaN(this.threshold)) {
        bus.$emit('heatingThresholdChanged', this.threshold)
      } else {
        bus.$emit('heatingThresholdChanged', 0)
      }
    },
    unitChange: function () {
      if (this.unit && !isNaN(this.unit)) {
        bus.$emit('maintenanceCostUnitChanged', this.unit)
      } else {
        bus.$emit('maintenanceCostUnitChanged', 0)
      }
    }
  }
}
</script>
