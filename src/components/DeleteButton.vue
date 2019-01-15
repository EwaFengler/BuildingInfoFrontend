<template>
  <div class="delete-button" v-on:click="deleteStructure">
    🗙
  </div>
</template>

<script>
import {HTTP} from '../http-common'
import {bus} from '../main'

export default {
  name: 'DeleteButton',
  props: {
    structureId: Number
  },
  methods: {
    deleteStructure: function (event) {
      HTTP.delete(`/structure/${this.structureId}`)
        .then(response => {
          if (response.status === 204) {
            bus.$emit('structuresUpdated')
          }
        })
        .catch(e => {
          console.log(e)
        })
    }
  }
}
</script>
