<template>
  <view>
    <view class="album_cover">
      <image :src="album.cover" mode="widthFix" />
      <view class="cover_bottom">
        <view class="cover_text">{{album.name}}</view>
        <view class="cover_fav">关注专辑</view>
      </view>
    </view>
    <view class="album_desc">
      <view class="album_user">
        <image class="user_avatar" mode="aspectFill" :src="album.user.avatar" />
        <view class="user_name">{{album.user.name}}</view>
      </view>
      <view class="desc_text">
        <text>{{album.desc}}</text>
      </view>
    </view>
    <view class="wallpaper_wrap">
      <view class="wrap_item" v-for="item in wallpaper" :key="item.id">
        <image :src="item.thumb+item.rule.replace('$<Height>','360')" mode="aspectFill" />
      </view>
    </view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      params: {
        limit: 30,
        order: "new",
        skip: 0,
        // 1: 有 album,表示第一次请求; 0: 无 album
        first: 1
      },
      id: "",
      album: {},
      wallpaper: []
    };
  },
  onLoad(option) {
    const { id } = option;
    // this.id = id;
    this.id = `5e5cf679e7bce739db1281e4`;
    this.getList();
  },
  methods: {
    getList: function() {
      uni.showLoading({
        title: "加载中"
      });
      uni
        .request({
          url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
          data: this.params
        })
        .then(data => {
          let [err, res] = data;
          uni.hideLoading();
          if (err === null) {
            console.log(res.data.res);
            this.album = res.data.res.album;
            this.wallpaper = res.data.res.wallpaper;
          } else {
            console.log(err);
          }
        });
    }
  }
};
</script>
<style lang='scss' scoped>
.album_cover {
  position: relative;
  .cover_bottom {
    position: absolute;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 750rpx;
    height: 80rpx;
    bottom: 0;
    left: 0;
    color: #fff;
    padding: 0 40rpx;
    .cover_text {
      font-size: 35rpx;
    }
    .cover_fav {
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: $color;
      width: 150rpx;
      height: 60rpx;
      border: 1rpx solid $color;
      border-radius: 10rpx;
    }
  }
}

.album_desc {
  padding: 50rpx 30rpx;
  .album_user {
    display: flex;
    // padding: 30rpx;
    .user_avatar {
      width: 80rpx;
      height: 80rpx;
    }
    .user_name {
      margin-left: 30rpx;
      font-size: 38rpx;
      color: #000;
    }
  }
  .desc_text {
    padding: 40rpx 0 20rpx 0;
  }
}

.wallpaper_wrap {
  display: flex;
  flex-wrap: wrap;
  .wrap_item {
    width: 33.3vw;
    height: 25vw;
    border: 3rpx solid #fff;
    image {
      width: 100%;
      height: 100%;
    }
  }
}
</style>