<template>
  <div>
    <z-puzzle @next="handleNext"  v-bind="puzzleConfig[level]"  />
  </div>
</template>

<script>
import ZPuzzle from "./Puzzle";
export default {
  components: {
    ZPuzzle
  },
  data() {
    return {
      level: 0,
      puzzleConfig: [
        { row: 3, col: 3, img: "./puzzleImg/1.jpg" },
        { row: 4, col: 4, img: "./puzzleImg/2.jpg" },
        { row: 5, col: 5, img: "./puzzleImg/3.jpg" },
        { row: 6, col: 6, img: "./puzzleImg/4.jpg" },
        { row: 8, col: 8, img: "./puzzleImg/5.jpg" }
      ]
    }
  },
  methods: {
    handleNext() {
      this.level++;
      if (this.level === this.puzzleConfig.length) {
        //++在前代表加完之后进行比较 ，++在后代表比较后再加
        const answerFlag = window.confirm("恭喜你，通关了，要再来一遍不？");
        if (answerFlag) {
          this.level = 0;
        }
      }
     
    }
  }
};
</script>>