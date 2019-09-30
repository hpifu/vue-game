<template>
  <v-card class="ma-2 pa-0" width="140" color="green lighten-4">
    <v-flex md12 class="text">{{title}}</v-flex>
    <transition name="custom-classes-transition">
      <v-layout align-center justify-center fill-height text-center row wrap class="pa-0 ma-0">
        <v-flex md12 class="text">{{value}}</v-flex>
      </v-layout>
    </transition>
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
  font-size: 1rem;
}
</style>

<script lang="ts">
import { Component, Prop, Vue, Watch, Provide } from "vue-property-decorator";

@Component
export default class Score extends Vue {
  @Prop({ default: "title" }) private title!: string;
  @Prop({ default: 0 }) private num!: number;
  @Prop({ default: "Left" }) private direction!: string;

  get value() {
    return this.num.toString();
  }

  get color() {
    if (this.num === 0) {
      return "green lighten-4";
    } else if (this.num === 2) {
      return "light-blue darken-1";
    } else if (this.num === 4) {
      return "cyan darken-1";
    } else if (this.num === 8) {
      return "teal darken-1";
    } else if (this.num === 16) {
      return "green darken-1";
    } else if (this.num === 32) {
      return "light-green darken-1";
    } else if (this.num === 64) {
      return "lime darken-1";
    } else if (this.num === 128) {
      return "yellow darken-1";
    } else if (this.num === 256) {
      return "amber darken-1";
    } else if (this.num === 512) {
      return "orange darken-1";
    } else if (this.num === 1024) {
      return "deep-orange darken-1";
    } else if (this.num === 2048) {
      return "brown darken-1";
    }

    return "green lighten-4";
  }
}
</script>