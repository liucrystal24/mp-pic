<template>
  <view>
    <scroll-view scroll-y enable-flex class="video_wrap" @scrolltolower="handleToLower">
      <view
        class="video_wrap_item"
        v-for="item in videoList"
        :key="item.id"
        @click="handleGoVideo(item)"
      >
        <image :src="item.img" mode="widthFix" />
      </view>
    </scroll-view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      params: {
        limit: 30,
        skip: 0,
        order: this.request.keyword
      },
      videoList: [],
      hasMore: true
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      uni.showLoading({
        title: "加载中"
      });
      uni
        .request({
          url: `http://157.122.54.189:9088/videoimg/v1/videowp${this.request.requesturl}`,
          data: this.params
        })
        .then(data => {
          let [err, res] = data;
          if (err === null) {
            console.log(res.data.res.videowp);
            uni.hideLoading();
            if (res.data.res.videowp.length !== 0) {
              this.videoList = [...this.videoList, ...res.data.res.videowp];
            } else {
              this.hasMore = false;
              uni.showToast({
                title: "没有图片了",
                icon: "none"
              });
            }
          }
        });
    },
    handleToLower() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        uni.showToast({
          title: "没有图片了",
          icon: "none"
        });
      }
    },
    handleGoVideo(item) {
      console.log(item);
      getApp().globalData.videoList = item;
      uni.navigateTo({
        url: "/pages/videoPlay/index"
      });
    }
  },
  props: {
    request: Object
  },
  watch: {
    request(n, o) {
      this.videoList = [];
      this.params.skip = 0;
      this.getList();
    }
  }
};
</script>
<style lang='scss' scoped>
.video_wrap {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .video_wrap_item {
    width: 33.33%;
    border: 5rpx solid #fff;
    image {
    }
  }
}
</style>