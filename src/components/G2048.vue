<template>
  <v-card flat outlined width="420" class="py-10 px-12">
    <v-layout justify-center row fill-height text-center>
      <template v-for="(_, i) in 4">
        <v-flex md3 v-for="(_, j) in 4" :key="i * n + j" class="pa-1" outlined tile>
          <Square :value="nums[i][j] == 0 ? '': nums[i][j]" />
        </v-flex>
      </template>
    </v-layout>
  </v-card>
</template>

<script lang="ts">
import { Component, Prop, Vue, Provide, Emit } from "vue-property-decorator";
import Square from "./Square.vue";

@Component({
  components: {
    Square,
  },
})
export default class G2048 extends Vue {
  @Provide() public nums: number[][] = [
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
  ];
  @Provide() public n: number = 4;

  public countZero(nums: number[][]) {
    let count = 0;
    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 4; j++) {
        if (nums[i][j] === 0) {
          count++;
        }
      }
    }
    return count;
  }

  public randInt(max: number) {
    return Math.floor(Math.random() * Math.floor(max));
  }

  public addNumTwo(nums: number[][]) {
    let count = this.randInt(this.countZero(nums));
    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 4; j++) {
        if (nums[i][j] === 0) {
          if (count === 0) {
            this.$set(nums[i], j, 2);
          }
          count--;
        }
      }
    }
  }

  public moveRight(nums: number[][]) {
    for (let i = 0; i < 4; i++) {
      let k = 3;
      for (let j = 3; j >= 0; j--) {
        if (nums[i][j] !== 0) {
          if (k !== j) {
            this.$set(nums[i], k--, nums[i][j]);
            this.$set(nums[i], j, 0);
          } else {
            k--;
          }
        }
      }
    }
  }

  public moveLeft(nums: number[][]) {
    for (let i = 0; i < 4; i++) {
      let k = 0;
      for (let j = 0; j < 4; j++) {
        if (nums[i][j] !== 0) {
          if (k !== j) {
            this.$set(nums[i], k++, nums[i][j]);
            this.$set(nums[i], j, 0);
          } else {
            k++;
          }
        }
      }
    }
  }

  public calLeft(nums: number[][]) {
    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 3; j++) {
        if (nums[i][j] === nums[i][j + 1]) {
          this.$set(nums[i], j, nums[i][j] * 2);
          this.$set(nums[i], j + 1, 0);
          j++;
        }
      }
    }
  }

  public moveUp(nums: number[][]) {
    for (let i = 0; i < 4; i++) {
      let k = 0;
      for (let j = 0; j < 4; j++) {
        if (nums[j][i] !== 0) {
          if (k !== j) {
            this.$set(nums[k++], i, nums[j][i]);
            this.$set(nums[j], i, 0);
          } else {
            k++;
          }
        }
      }
    }
  }

  public moveDown(nums: number[][]) {
    for (let i = 0; i < 4; i++) {
      let k = 3;
      for (let j = 3; j >= 0; j--) {
        if (nums[j][i] !== 0) {
          if (k !== j) {
            this.$set(nums[k--], i, nums[j][i]);
            this.$set(nums[j], i, 0);
          } else {
            k--;
          }
        }
      }
    }
  }

  public Right() {
    this.moveRight(this.nums);
  }

  public Left() {
    this.moveLeft(this.nums);
    this.calLeft(this.nums);
  }

  public Up() {
    this.moveUp(this.nums);
  }

  public Down() {
    this.moveDown(this.nums);
  }

  public mounted() {
    window.addEventListener("keymoveDown", (e) => {
      e.preventDefault();
      if (e.key === "ArrowLeft") {
        // console.log("moveLeft");
        this.Left();
        this.addNumTwo(this.nums);
      }
      if (e.key === "ArrowRight") {
        // console.log("moveRight");
        this.Right();
        this.addNumTwo(this.nums);
      }
      if (e.key === "ArrowDown") {
        // console.log("moveDown");
        this.Down();
        this.addNumTwo(this.nums);
      }
      if (e.key === "ArrowUp") {
        // console.log("moveUp");
        this.Up();
        this.addNumTwo(this.nums);
      }
      e.stopPropagation();
    });

    for (let i = 0; i < 3; i++) {
      this.addNumTwo(this.nums);
    }
  }
}
</script>