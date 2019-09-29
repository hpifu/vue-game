<template>
  <v-card class="ma-2" width="90" height="90" :color="color">
    <v-layout align-center justify-center fill-height text-center row wrap class="pa-0 ma-0">
      <v-flex class="text" :style="{ 'font-size': fontSize }">{{value}}</v-flex>
    </v-layout>
  </v-card>
</template>

<style scoped>
div.square-2048 {
  border: 1px solid rgba(0, 0, 0, 0.12);
  margin: 0;
  position: relative;
}

div.text {
  font-family: "Concert One";
  color: aliceblue;
}
</style>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Square extends Vue {
  @Prop({ default: "" }) private value!: string;

  get fontSize() {
    if (this.value.length < 3) {
      return "4rem";
    } else if (this.value.length < 4) {
      return "3rem";
    }
    return "2.2rem";
  }

  get color() {
    if (this.value === "") {
      return "green lighten-4";
    } else if (this.value === "2") {
      return "light-blue darken-1";
    } else if (this.value === "4") {
      return "cyan darken-1";
    } else if (this.value === "8") {
      return "teal darken-1";
    } else if (this.value === "16") {
      return "green darken-1";
    } else if (this.value === "32") {
      return "light-green darken-1";
    } else if (this.value === "64") {
      return "lime darken-1";
    } else if (this.value === "128") {
      return "yellow darken-1";
    } else if (this.value === "256") {
      return "amber darken-1";
    } else if (this.value === "512") {
      return "orange darken-1";
    } else if (this.value === "1024") {
      return "deep-orange darken-1";
    } else if (this.value === "2048") {
      return "brown darken-1";
    }

    return "green lighten-4";
  }
}
</script>