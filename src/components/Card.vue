<template>
  <div class="card" v-bind:class="structureTypes[0]">
    <div>
      <p>{{ structure.name }}</p>
      <DeleteButton v-bind:structureId="structure.id"/>
      <Properties v-bind:structureId="structure.id"/>
    </div>
    <div v-if="structureTypes.length > 1" class="flex-container">
      <Card v-for="structure in structure.structures"
        :key="structure.id"
        v-bind:structure="structure"
        v-bind:structureTypes="structureTypes.slice(1)"/>
    </div>
  </div>
</template>

<script>
import Properties from './Properties'
import DeleteButton from './DeleteButton'

export default {
  name: 'Card',
  components: {
    Properties,
    DeleteButton
  },
  props: {
    structureTypes: Array,
    structure: Object
  },
  data () {
    return {
    }
  }
}
</script>
