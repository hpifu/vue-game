<template>
  <v-card class="ma-1 square-2048" width="90" height="90">
    <div class="text">{{value}}</div>
  </v-card>
</template>

<style scoped>
div.square-2048 {
  background-color: coral;
  border: 1px solid rgba(0, 0, 0, 0.12);
  margin: 0;
  position: relative;
}
div.text {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  font-size: 4rem;
  font-family: "Changa";
  color: aliceblue;
}
</style>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Square extends Vue {
  @Prop({ default: "title" }) private value!: string;

  public mounted() {
    // document.getElementById("square").style.width = document.getElementById(
    //   "container"
    // ).offsetHeight;
  }
}
</script>