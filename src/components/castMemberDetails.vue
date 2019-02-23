<template lang="html">
  <div class="filmDetails">
    <h3>{{castMember.name}}</h3>
    <p>Gender: {{castMember.gender}}</p>
    <p>Age: {{castMember.age}}</p>
    <p>Eye Colour: {{castMember.eye_color}}</p>
    <p>Hair Colour: {{castMember.hair_color}}</p>
    <button type="button" name="ViewFilmography" v-on:click="handleFilmListClick">View Filmography</button> </p>
    <button type="button" name="ViewSpecies" v-on:click="handleSpeciesClick">View Species</button> </p>
  </div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  name: 'castMemberDetails',
  props: ['castMember'],
  methods:{
    handleFilmListClick() {
      eventBus.$emit("film-list", this.castMember);
    },
    handleSpeciesClick(){
      eventBus.$emit("species-list", this.castMember);
    }
  }
}

</script>

<style lang="css" scoped>
.filmDetails{
  width: 50%;
}
</style>
