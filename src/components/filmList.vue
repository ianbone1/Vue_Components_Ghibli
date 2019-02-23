<template lang="html">
  <div class="FilmList">
    <h3>Films</h3>
    <ul>
      <film-list-item v-for="(film, index) in filmList" :film="film" :key="index"></film-list-item>
    </ul>
  </div>
</template>

<script>
import filmListItem from './filmListItem.vue';
export default {
    name: 'film-list',
    props: ['filmList'],
    components: {
      "film-list-item": filmListItem
    },
}
</script>

<style lang="css" scoped>
.FilmsList{
  width:50%;
}
</style>
