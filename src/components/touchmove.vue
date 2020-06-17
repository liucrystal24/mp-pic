<template>
  <view @touchstart="handleStart" @touchend="handleEnd">
    <slot></slot>
  </view>
</template>
<script>
export default {
  data() {
    return {
      startX: "",
      startY: "",
      endX: "",
      endY: "",
      startDate: "",
      endDate: "",
      indexstep: 0
    };
  },
  onLoad() {},
  methods: {
    handleStart(e) {
      // 点击时的横纵坐标
      this.startX = e.changedTouches[0].clientX;
      this.startY = e.changedTouches[0].clientY;
      this.startDate = Date.now();
    },
    handleEnd(e) {
      // 离开时的横纵坐标
      this.endX = e.changedTouches[0].clientX;
      this.endY = e.changedTouches[0].clientY;
      this.endDate = Date.now();
      let dateDis = this.endDate - this.startDate;
      // 手指左右移动超过 10 个单位,上下移动不超过 10 个单位 且 时间少于 3s 才开始判断
      if (Math.abs(this.endX - this.startX) > 10 && Math.abs(this.endY - this.startY) < 10 && dateDis < 3000) {
        // indexChange 子传父 +1/-1
        let indexChange = this.endX - this.startX > 0 ? -1 : 1;
        this.$emit("touchResult", indexChange);
        return;
      }
      this.$emit("touchResult", 0);
    }
  }
};
</script>
<style lang='scss'>
</style>