<template>
  <view class="video_container">
    <image :src="videoList.img" />
    <view class="video_tools">
      <view :class="['iconfont',muted?'icon-mute':'icon-soundsize']" @click="handleSound()"></view>
      <view class="iconfont icon-fenxiang">
        <button open-type="share">zhuanfa</button>
      </view>
    </view>
    <view class="video_content">
      <video :src="videoList.video" objectFit="fill" :muted="muted"></video>
    </view>
    <view class="video_download">
      <view class="download_button" @click="handleDownload">下载高清</view>
    </view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      videoList: {},
      muted: false,
      sound_style: "iconfont icon-soundsize"
    };
  },
  onLoad() {
    this.videoList = getApp().globalData.videoList;
  },
  methods: {
    handleSound() {
      this.muted = !this.muted;
    },
    async handleDownload() {
      uni.showLoading({
        title: "下载中"
      });
      const [err, res] = await uni
        .downloadFile({
          url: this.videoList.video
        })
        .catch(err => {
          console.err(err);
        });
      uni.hideLoading();
      if (err === null) {
        const result = await uni.saveVideoToPhotosAlbum({
          filePath: res.tempFilePath
        });
        if (result[0] === null) {
          uni.showToast({
            title: "下载成功"
          });
        } else {
          uni.showToast({
            title: "下载失败",
            icon: "none"
          });
        }
      }
    }
  }
};
</script>
<style lang='scss' scoped>
@import "@/style/video.wxss";
.video_container {
  position: relative;
  image {
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: -1;
    filter: blur(20px);
  }
  .video_tools {
    height: 80rpx;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    .iconfont {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 30rpx;
      color: #fff;
      width: 60rpx;
      height: 60rpx;
      border-radius: 30rpx;
      font-size: 40rpx;
      background-color: rgba(0, 0, 0, 0.5);
      button {
        position: absolute;
        width: 100%;
        height: 100%;
        border-radius: 30rpx;
        // display: none;
        opacity: 0;
      }
    }
  }

  .video_content {
    // width: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    video {
      width: 360rpx;
      height: 600rpx;
    }
  }
  .video_download {
    padding: 20rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    .download_button {
      width: 360rpx;
      height: 80rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      background-color: rgba(0, 0, 0, 0.6);
      border: 2rpx solid #fff;
      border-radius: 20rpx;
    }
  }
}
</style>