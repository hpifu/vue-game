<template>
  <v-card flat outlined width="460" class="pa-2" color="blue-grey">
    <v-layout align-center justify-center fill-height text-center class="px-0 py-6 ma-0">
      <v-flex md6>
        <v-layout align-center justify-center fill-height text-center class="pa-0 ma-0">
          <Score :num="score" title="score" />
        </v-layout>
      </v-flex>
      <v-flex md6>
        <v-layout align-center justify-center fill-height text-center class="pa-0 ma-0">
          <Score :num="best" title="best" />
        </v-layout>
      </v-flex>
    </v-layout>
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

    <v-dialog v-model="show" persistent max-width="230" transition color="rgb(0, 0, 0, 0)">
      <v-card color="rgb(0, 0, 0, 0.5)">
        <v-layout align-center justify-center fill-height text-center row wrap class="pa-6 ma-0">
          <v-flex md12 class="text ma-6">Game Over</v-flex>
          <v-flex md12>
            <v-btn text color="light-blue lighten-5" @click="newGame()">重新开始</v-btn>
          </v-flex>
        </v-layout>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<style scoped>
div.text {
  font-family: "Concert One";
  color: #e1f5fe;
  font-size: 1rem;
}
</style>

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
import Score from "./Score.vue";
import { TweenLite } from "gsap";

@Component({
  components: {
    Square,
    Score
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
  @Provide() public score: number = 0;
  @Provide() public best: number = 0;
  @Provide() public show: boolean = false;

  public newGame() {
    this.show = false;
    for (let i = 0; i < this.n; i++) {
      for (let j = 0; j < this.n; j++) {
        this.nums[i][j] = 0;
      }
    }
    for (let i = 0; i < 3; i++) {
      this.addNumTwo(this.nums);
    }
    this.score = 0;
  }

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

  public gameOver(): boolean {
    return (
      this.countZero(this.nums) === 0 &&
      !this.canMove(this.nums, this.left) &&
      !this.canMove(this.nums, this.right) &&
      !this.canMove(this.nums, this.up) &&
      !this.canMove(this.nums, this.down)
    );
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

          this.score += nums[x1][y1];
          if (this.best < this.score) {
            this.best = this.score;
          }
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
        if (this.gameOver()) {
          this.show = true;
        }
      }
      e.stopPropagation();
    });

    this.newGame();
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