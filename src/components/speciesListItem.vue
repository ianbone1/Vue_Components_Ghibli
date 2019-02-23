<template lang="html">
  <div>
  <li v-on:click="handleClick" v-if="speciesMember!=null">
   <a>{{speciesMember.name}}</a>
  </li>
</div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  name: 'speciesListItem',
  props: ['speciesMember'],
  methods:{
    handleClick() {
      eventBus.$emit("speciesMember-selected", this.speciesMember);
    }
  }
}
</script>

<style lang="css" scoped>
li{
width:100%;
}
</style>
