<template lang="html">
  <div>
  <li v-on:click="handleClick" v-if="castMember!=null">
   <a>{{castMember.name}}</a>
  </li>
</div>
</template>

<script>
import { eventBus } from "../main.js";
export default {
  name: 'castListItem',
  props: ['castMember'],
  methods:{
    handleClick() {
      eventBus.$emit("castMember-selected", this.castMember);
    }
  }
}
</script>

<style lang="css" scoped>
li{
width:100%;
}
</style>
