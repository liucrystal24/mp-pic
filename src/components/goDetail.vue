<!--
 * @Author: ChrisLiu
 * @Date: 2020-06-13 21:12:12
 * @LastEditors: ChrisLiu
 * @LastEditTime: 2020-11-12 20:01:41
 * @Description: file content
-->
<template>
  <view @click="handleCLick">
    <slot></slot>
  </view>
</template>
<script>
export default {
  data() {
    return {};
  },
  props: {
    list: Array,
    index: Number
  },
  mounted() {},
  methods: {
    handleCLick() {
      // 保存数组和索引
      getApp().globalData.imgList = this.list;
      getApp().globalData.imgIndex = this.index;
      // 跳转至图片详情
      uni.navigateTo({
        url: "/pages/imgDetail/index"
      });
    }
  }
};
</script>
<style lang='scss'>
</style>