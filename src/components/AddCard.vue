<template>
  <div class="card" v-bind:class="structureType">
    <p>🞣 Add new {{ structureType }}</p>
  </div>
</template>

<script>

export default {
  name: 'AddCard',
  props: {
    structureType: String
  }
}
</script>
