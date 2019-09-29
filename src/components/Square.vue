<template>
  <div class="square-2048">
    <div class="text">{{value}}</div>
  </div>
</template>

<style scoped>
div.square-2048 {
  background-color: cyan;
  border: 1px solid rgba(0, 0, 0, 0.12);
  padding-top: 100%;
  margin: 0;
  position: relative;
}
div.text {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  transform: translateY(40%);
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