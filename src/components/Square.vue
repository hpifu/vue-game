<template>
  <v-card class="ma-2 pa-0" width="90" height="90" color="green lighten-4">
    <transition
      name="custom-classes-transition"
      :enter-active-class="enterAnimate"
      :leave-active-class="leaveAnimate"
    >
      <v-card v-if="num != 0" class="ma-0 pa-0" width="90" height="90" :color="color">
        <v-layout align-center justify-center fill-height text-center row wrap class="pa-0 ma-0">
          <v-flex class="text" :style="{ 'font-size': fontSize }">{{value}}</v-flex>
        </v-layout>
      </v-card>
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
}
</style>

<script lang="ts">
import { Component, Prop, Vue, Watch, Provide } from "vue-property-decorator";
import { TweenMax, TweenLite } from "gsap";

@Component
export default class Square extends Vue {
  @Prop({ default: 0 }) private num!: number;
  @Provide() private tweenedNumber: number = 0;
  @Prop({ default: "Left" }) private direction!: string;

  get value() {
    return this.num === 0 ? "" : this.num.toString();
    return this.num === 0 ? "" : this.tweenedNumber.toFixed(0);
  }

  get enterAnimate() {
    return "animated zoomIn";
    if (this.direction === "Left") {
      return "animated zoomInRight";
    }
    if (this.direction === "Right") {
      return "animated zoomInLeft";
    }
    if (this.direction === "Down") {
      return "animated zoomInUp";
    }
    if (this.direction === "Up") {
      return "animated zoomInDown";
    }
  }

  get leaveAnimate() {
    return "animated zoomOut";
    if (this.direction === "Left") {
      return "animated zoomOutLeft";
    }
    if (this.direction === "Right") {
      return "animated zoomOutRight";
    }
    if (this.direction === "Down") {
      return "animated zoomOutDown";
    }
    if (this.direction === "Up") {
      return "animated zoomOutUp";
    }
  }

  @Watch("num")
  public onValueChange(val: string) {
    TweenLite.to(this.$data, 0.5, { tweenedNumber: val });
  }

  get fontSize() {
    if (this.num.toString().length < 3) {
      return "4rem";
    } else if (this.num.toString().length < 4) {
      return "3rem";
    }
    return "2.2rem";
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