<template lang="html">
  <div class="castList">
    <h3>Cast</h3>
    <ul>
      <cast-list-item v-for="(castMember, index) in cast" :castMember="castMember" :key="index"></cast-list-item>
    </ul>
  </div>
</template>

<script>
import castListItem from './castListItem.vue';
export default {
    name: 'cast-list',
    props: ['cast'],
    components: {
      "cast-list-item": castListItem
    },
}
</script>

<style lang="css" scoped>
</style>
