<template>
  <v-card flat outlined width="460" class="pa-2" color="blue-grey">
    <v-layout wrap class="pa-0 ma-0">
      <template v-for="(_, i) in n">
        <v-flex xs12 :key="i">
          <v-layout align-center justify-center fill-height text-center class="pa-0 ma-0">
            <template v-for="(_, j) in n">
              <Square :key="i * n + j" :num="nums[i][j]" :direction="direction" />
            </template>
          </v-layout>
        </v-flex>
      </template>
    </v-layout>
  </v-card>
</template>

<script lang="ts">
import {
  Component,
  Prop,
  Vue,
  Provide,
  Emit,
  Watch
} from "vue-property-decorator";
import Square from "./Square.vue";
import { TweenLite } from "gsap";

@Component({
  components: {
    Square
  }
})
export default class G2048 extends Vue {
  @Provide() public nums: number[][] = [
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0]
  ];
  @Provide() public n: number = 4;
  @Provide() public direction: string = "Left";

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

  public canMove(
    nums: number[][],
    rot: (x: number, y: number) => number[]
  ): boolean {
    for (let i = 0; i < 4; i++) {
      let k = 0;
      for (let j = 0; j < 4; j++) {
        const [x1, y1] = rot(i, j);
        if (nums[x1][y1] !== 0) {
          if (k !== j) {
            return true;
            k++;
          } else {
            k++;
          }
        }
      }
    }

    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 3; j++) {
        const [x1, y1] = rot(i, j);
        const [x2, y2] = rot(i, j + 1);
        if (nums[x1][y1] === 0) {
          break;
        }
        if (nums[x1][y1] === nums[x2][y2]) {
          return true;
          j++;
        }
      }
    }

    return false;
  }

  public move(nums: number[][], rot: (x: number, y: number) => number[]) {
    for (let i = 0; i < 4; i++) {
      let k = 0;
      for (let j = 0; j < 4; j++) {
        const [x1, y1] = rot(i, j);
        if (nums[x1][y1] !== 0) {
          if (k !== j) {
            const [x2, y2] = rot(i, k);
            this.$set(nums[x2], y2, nums[x1][y1]);
            this.$set(nums[x1], y1, 0);
            k++;
          } else {
            k++;
          }
        }
      }
    }
  }

  public merge(nums: number[][], rot: (x: number, y: number) => number[]) {
    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 3; j++) {
        const [x1, y1] = rot(i, j);
        const [x2, y2] = rot(i, j + 1);
        if (nums[x1][y1] === 0) {
          break;
        }
        if (nums[x1][y1] === nums[x2][y2]) {
          this.$set(nums[x1], y1, nums[x1][y1] * 2);
          this.$set(nums[x2], y2, 0);
          j++;
        }
      }
    }
  }

  public mounted() {
    window.addEventListener("keydown", (e) => {
      e.preventDefault();
      const rotMap: Map<string, (x: number, y: number) => number[]> = new Map([
        ["ArrowLeft", this.left],
        ["ArrowRight", this.right],
        ["ArrowDown", this.down],
        ["ArrowUp", this.up]
      ]);
      if (rotMap.has(e.key)) {
        const rot = rotMap.get(e.key)!;
        if (this.canMove(this.nums, rot)) {
          this.direction = e.key.substr(5);
          this.move(this.nums, rot);
          this.merge(this.nums, rot);
          this.move(this.nums, rot);
          this.addNumTwo(this.nums);
        }
      }
      e.stopPropagation();
    });

    for (let i = 0; i < 3; i++) {
      this.addNumTwo(this.nums);
    }
  }

  private left(x: number, y: number) {
    return [x, y];
  }

  private up(x: number, y: number) {
    return [y, x];
  }

  private right(x: number, y: number) {
    return [x, this.n - y - 1];
  }

  private down(x: number, y: number) {
    return [this.n - y - 1, x];
  }
}
</script>