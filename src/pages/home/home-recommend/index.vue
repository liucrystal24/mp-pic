<template>
  <view>
    <view>推荐</view>
    <view class="recommend_wrap">
      <view class="recommend_item" v-for="item in recommendList" :key="item.id">
        <image :src="item.thumb" mode='widthFix' />
      </view>
    </view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      // 推荐列表
      recommendList: []
      // 热门
    };
  },
  mounted() {
    uni
      .request({
        url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
        data: {
          limit: 30,
          order: "hot",
          skip: 0
        }
      })
      .then(data => {
        let [err, res] = data;
        if (err === null) {
          this.recommendList = res.data.res.homepage[1].items;
          console.log(this.recommendList);
        } else {
          console.log(err);
        }
      });
  },
  methods: {}
};
</script>
<style lang="scss" scoped>
.recommend_wrap {
  display: flex;
  flex-wrap: wrap;
  .recommend_item {
    width: 50%;
    border:5rpx solid #fff;
  }
}
</style>