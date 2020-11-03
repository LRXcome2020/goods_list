<!--
 * @,@Author: ,: your name
 * @,@Date: ,: 2020-11-02 15:47:55
 * @,@LastEditTime: ,: 2020-11-02 16:30:16
 * @,@LastEditors: ,: your name
 * @,@Description: ,: In User Settings Edit
 * @,@FilePath: ,: \goods\vue_dewu\src\pages\Classify\components\Alphabet.vue
 -->
<template>
  <ul class="list">
    <li
      class="item"
      v-for="item of letters"
      :key="item"
      :ref="item"
      @click="handleLetterClick"
      @touchstart="handleLetterStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      {{ item }}
    </li>
  </ul>
</template>
<script>
export default {
  name: "ClassifyAiphabet",
  props: {
    Brands: Object,
  },
  data() {
    return {
      // 标识位，只有在touchstart之后才能touchmove
      touchStatus: false,
      startY: 0,
      timer: 0,
    };
  },
  updated() {
    this.startY = this.$refs["A"][0].offsetTop;
  },
  computed: {
    letters() {
      const letters = [];
      for (let i in this.Brands) {
        letters.push(i);
      }
      return letters;
    },
  },
  methods: {
    handleLetterClick(e) {
      //向外触发事件
      this.$emit("change", e.target.innerText);
      // console.log(e.target.innerText);
    },
    handleLetterStart() {
      this.touchStatus = true;
    },
    handleTouchMove(e) {
      if (this.touchStatus) {
        if (this.timer) {
          clearTimeout(this.timer);
        }
        this.timer = setTimeout(() => {
          const startY = this.$refs["A"][0].offsetTop;
          console.log("开始得距离",startY)
          const touchY = e.touches[0].clientY - 79;
           console.log("移动得距离",touchY)
          //  当前滑动的位置，对应字母的下标
          const index = Math.floor((touchY - startY) / 20);
          if (index >= 0 && index < this.letters.length) {
            this.$emit("change", this.letters[index]);
          }
          // console.log(index);
        }, 16);
      }
    },
    handleTouchEnd() {
      this.touchStatus = false;
    },
  },
};
</script>
<style lang="stylus" scoped>
.list {
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: absolute;
  right: 0;
  top: 2rem;
  bottom: 0;
  width: 0.4rem;

  .item {
    line-height: 0.4rem;
    text-align: center;
    color: $bgcolor;
    font-weight: 600;
  }
}
</style>