<template lang="html">
  <div class="speciesList">
    <h3>Species</h3>
    <ul>
      <species-list-item v-for="(speciesMember, index) in speciesList" :speciesMember="speciesMember" :key="index"></species-list-item>
    </ul>
  </div>
</template>

<script>
import speciesListItem from './speciesListItem.vue';
export default {
    name: 'species-list',
    props: ['speciesList'],
    components: {
      "species-list-item": speciesListItem
    },
}
</script>

<style lang="css" scoped>
</style>
