<!--
 * @Author: ChrisLiu
 * @Date: 2020-06-02 22:57:25
 * @LastEditors: ChrisLiu
 * @LastEditTime: 2020-11-14 22:36:12
 * @Description: file content
-->
<template>
  <scroll-view scroll-y class="category_list">
    <view class="category_wrap">
      <navigator
        class="category_item"
        v-for="item in category"
        :key="item.id"
        :url="`/pages/imgCategory/index?id=${item.id}`"
      >
        <image :src="item.cover" mode="aspectFill" />
        <view class="category_name">{{item.name}}</view>
      </navigator>
    </view>
  </scroll-view>
</template>
<script>
export default {
  data() {
    return {
      category: []
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "分类"
    });
    this.getList();
  },
  methods: {
    getList() {
      uni
        .request({
          url: "https://service.picasso.adesk.com/v1/vertical/category"
        })
        .then(data => {
          let [err, res] = data;
          console.log(res.data.res.category);
          let nowYear = new Date().getFullYear();
          let nowMonth = new Date().getMonth() + 1;
          let nowDate = new Date().getDate();
          // console.log(nowYear, nowMonth, nowDate);
          if (
            (nowYear === 2020) &
            (nowMonth === 11) &
            (nowDate === 14 || nowDate === 15 || nowDate === 16)
          ) {
            let categoryFilter = res.data.res.category.filter(item => {
              return (
                item.name !== "美女" &&
                item.name !== "男人" &&
                item.name !== "明星"
              );
            });
            this.category = categoryFilter;
          } else {
            this.category = res.data.res.category;
          }
        });
    }
  }
};
</script>
<style lang="scss">
.category_list {
  height: calc(100vh - 36px);
  padding: 10rpx;
  .category_wrap {
    display: flex;
    flex-wrap: wrap;
    .category_item {
      width: 33.33%;
      border: 5rpx solid #fff;
      position: relative;
      image {
        height: calc(750rpx / 3);
      }
      .category_name {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 40rpx;
        position: absolute;
        bottom: 0;
        color: #fff;
        background-image: linear-gradient(
          to right top,
          rgba(0, 0, 0, 0.4),
          rgba(0, 0, 0, 0)
        );
        font-size: 30rpx;
      }
    }
  }
}
</style>
