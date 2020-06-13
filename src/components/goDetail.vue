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
    index: String
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