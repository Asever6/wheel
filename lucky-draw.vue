<template>
  <div style="width:500px;height:500px;padding:40px">
    <div class="square">
      <ul class="square-inner flex">
        <li
          v-for="(item, index) in awards"
          :key="index"
          :class="[{ active: index == currentIndex }]"
        >
          <template v-if="index == 4">按钮</template>
          <template v-else> {{ index }}</template>
        </li>
      </ul>
    </div>
    <button @click="startLottery">开始抽奖</button>
  </div>
</template>

<script>
export default {
  name: "father",
  data() {
    return {
      awards: [
        // 图片名字与ID
        { id: 1, name: 10 },
        { id: 2, name: 100 },
        { id: 3, name: 2 },
        { id: 4, name: 1 },
        { id: 5, name: 5 },
        { id: 6, name: 50 },
        { id: 7, name: 0 },
        { id: 8, name: 5 },
        { id: 9, name: 6 }
      ],
      list: [0, 1, 2, 5, 8, 7, 6, 3],
      timer: null, // 定时器
      index: 0, //起始位置
      currentIndex: 0, //转动起始位置
      prize: 5, //中奖位置
      times: 0, // 转动跑格子次数,
      cycle: 60, // 转动基本次数：即至少需要转动多少次再进入抽奖环节
      speed: 200 // 初始转动速度
    };
  },
  methods: {
    startLottery() {
      this.startRoll();
    },
    // 开始转动
    startRoll() {
      this.times += 1; // 转动次数
      this.oneRoll(); // 转动过程调用的每一次转动方法，这里是第一次调用初始化
      this.usePrize();
    },
    oneRoll() {
      this.index++;
      if (this.index >= 8) {
        this.index = 0;
      }
      this.currentIndex = this.list[this.index];
    },
    usePrize() {
      if (this.times > this.cycle && this.currentIndex == this.prize) {
        clearTimeout(this.timer);
        this.times = 0;
        this.index = 0;
        // this.currentIndex = 0;
        return this.$message.warning("恭喜你中奖了");
      } else {
        if (this.times < this.cycle - 20) {
          this.speed -= 4; // 加快转动速度
        } else {
          this.speed += 10; // 抽奖即将结束，放慢转动速度
        }
        this.timer = setTimeout(this.startRoll, this.speed);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.square {
  position: relative;
  width: 100%;
  height: 0;
  padding-bottom: 100%; /* padding百分比是相对父元素宽度计算的 */
  margin-bottom: 30px;
}
.square-inner {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%; /* 铺满父元素容器，这时候宽高就始终相等了 */
}
.square-inner > li {
  width: calc(98% / 3); /* calc里面的运算符两边要空格 */
  height: calc(98% / 3);
  margin-right: 1%;
  margin-bottom: 1%;
  overflow: hidden;
}
.flex {
  display: flex;
  flex-wrap: wrap;
}
.flex > li {
  flex-grow: 1; /* 子元素按1/n的比例进行拉伸 */
  /* background-color: #4d839c; */
  text-align: center;
  border: 1px solid #f00;
  /* color: #f00; */
  font-size: 50px;
  line-height: 2;
}
.flex > li:nth-of-type(3n) {
  /* 选择个数是3的倍数的元素 */
  margin-right: 0;
}
.flex > li:nth-of-type(n + 7) {
  /* 选择倒数的三个元素，n可以取0 */
  margin-bottom: 0;
}
.active {
  background-color: burlywood;
}
</style>
