<template lang="html">
  <div class="filmDetails">
    <h3>{{film.title}}</h3>
    <p>{{film.description}}</p>
    <p>Director: {{film.director}}</p>
    <p>Producer: {{film.producer}}</p>
    <p>Release Date: {{film.release_date}}</p>
    <p> <button type="button" name="ViewCast" v-on:click="handleCastClick">View Cast</button> </p>
  </div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  name: 'filmDetails',
  props: ['film'],
  methods:{
    handleCastClick() {
      eventBus.$emit("film-cast", this.film);
    }
  }
}

</script>

<style lang="css" scoped>
.filmDetails{
  width: 50%;
}
</style>
