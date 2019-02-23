<template lang="html">
  <div class="speciesDetails">
    <h3>{{species.name}}</h3>
    <p>Classification: {{species.classification}}</p>
    <p>Eye Colours: {{species.eye_colors}}</p>
    <p>Hair Colours: {{species.hair_colors}}</p>
    <button type="button" name="ViewSpeciesMembers" v-on:click="handleSpeciesListClick">View Members</button> </p>
    <!-- <button type="button" name="ViewSpeciesFilms" v-on:click="handleSpeciesFilmsClick">Film Appearances</button> </p> -->
  </div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  name: 'speciesMemberDetails',
  props: ['species'],
  methods:{
    handleSpeciesListClick() {
      eventBus.$emit("species-members-list", this.species);
    },
    // handleSpeciesFilmsClick(){
    //   // still to be handled
    //   eventBus.$emit("species-members-list", this.species);
    //   handleSpeciesFilmsClick
    // }
  }
}

</script>

<style lang="css" scoped>
.filmDetails{
  width: 50%;
}
</style>
