<template>
  <scroll-view scroll-y class="album_scroll" @scrolltolower="handleLower">
    <view class="banner_swiper">
      <swiper :indicator-dots="true" :autoplay="true" :circular="true">
        <swiper-item v-for="item in banner" :key="item.id">
          <image :src="item.thumb" />
        </swiper-item>
      </swiper>
    </view>
    <view class="album_list">
      <navigator
        class="album_item"
        v-for="item in album"
        :key="item.id"
        :url="`/pages/album/index?id=${item.id}`"
      >
        <view class="item_cover">
          <image :src="item.cover" mode="aspectFill" />
        </view>
        <view class="item_detail">
          <view class="item_name">{{item.name}}</view>
          <view class="item_desc text_one">{{item.desc}}</view>
          <view class="item_isfav">
            <view class="isfav_btn">+ 关注</view>
          </view>
        </view>
      </navigator>
    </view>
  </scroll-view>
</template>
<script>
export default {
  data() {
    return {
      params: {
        limit: 20,
        order: "new",
        skip: 0
      },
      banner: [],
      album: [],
      hasMore: true
    };
  },
  mounted() {
    this.getList();

    uni.setNavigationBarTitle({
      title: "专辑"
    });
  },
  methods: {
    getList: function() {
      uni.showLoading({
        title: "加载中"
      });
      uni
        .request({
          url: "http://service.picasso.adesk.com/v1/wallpaper/album",
          data: this.params
        })
        .then(data => {
          let [err, res] = data;
          uni.hideLoading();
          if (err === null) {
            console.log(res.data.res);
            if (this.banner.length === 0) {
              this.banner = res.data.res.banner;
            }
            if (res.data.res.album.length === 0) {
              this.hasMore = false;
              uni.showToast({
                title: "没有数据了",
                icon: "none"
              });
              return;
            }
            this.album = [...this.album, ...res.data.res.album];
          } else {
            console.log(err);
          }
        });
    },
    handleLower() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList();
        // console.log('sss')
      } else {
        uni.showToast({
          title: "已经没有数据了",
          icon: "none"
          // duration: 2000
        });
      }
    }
  }
};
</script>
<style lang="scss">
.album_scroll {
  height: calc(100vh - 36px);
}
.banner_swiper {
  swiper {
    height: calc(750rpx / 2.3);
    image {
      height: 100%;
    }
  }
}
.album_list {
  .album_item {
    display: flex;
    border-bottom: 1rpx solid #ddd;
    .item_cover {
      // width: 250rpx;
      flex: 1;
      padding: 20rpx;
      image {
        height: 200rpx;
        width: 200rpx;
      }
    }
    .item_detail {
      // width: 500rpx;
      flex: 2;
      overflow: hidden;
      padding: 20rpx;
      position: relative;
      .item_name {
        color: #000;
        font-weight: 600;
        font-size: 30rpx;
      }
      .item_desc {
        margin-top: 10rpx;
        font-size: 24rpx;
        padding: 10rpx 0;
      }

      .item_isfav {
        display: flex;
        justify-content: flex-end;
        padding: 10rpx;
        .isfav_btn {
          padding: 10rpx;
          border: 1rpx solid $color;
          // border-radius: 15rpx;
          color: $color;
        }
      }
    }
  }
}
.text_one {
  text-overflow: ellipsis;
  overflow: hidden;
  // 不换行
  white-space: nowrap;
}
</style>
