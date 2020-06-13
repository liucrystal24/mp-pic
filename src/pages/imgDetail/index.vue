<template>
  <view>
    <view class="pic_detail">
      <view class="pic_title">
        <view class="pic_avatar">
          <image :src="imgCurrent.user.avatar" mode="widthFix" />
        </view>
        <view class="pic_user">
          <view class="pic_username">{{imgCurrent.user.name}}</view>
          <view class="pic_time">{{dateDis}}</view>
        </view>
      </view>
      <view class="pic">
        <image :src="imgsrc" mode="widthFix" />
      </view>
      <view class="pic_info">
        <view class="pic_rank">
          <text class="iconfont icon-dianzan"></text>
          <text class="pic_ranktext">{{imgCurrent.rank}}</text>
        </view>
        <view class="pic_fav">
          <text class="iconfont icon-shoucang1"></text>
          <text class="pic_favtext">收藏</text>
        </view>
      </view>
    </view>
  </view>
</template>
<script>
import moment from "moment";
moment.locale("zh-cn");
export default {
  data() {
    return {
      // imgList: [],
      // imgIndex: 0,
      imgCurrent: {},
      imgsrc: "",
      dateDis: ""
    };
  },
  onLoad() {
    // this.imgList = getApp().globalData.imgList;
    // this.imgIndex = getApp().globalData.imgIndex;
    const { imgList, imgIndex } = getApp().globalData;
    this.imgCurrent = imgList[imgIndex];
    // 修改图片格式
    this.imgsrc =
      this.imgCurrent.thumb + this.imgCurrent.rule.replace("$<Height>", 360);

    // 修改日期
    this.dateDis = moment(this.imgCurrent.atime * 1000).fromNow();
  },
  methods: {}
};
</script>
<style lang='scss' scoped>
.pic_detail {
  .pic_title {
    display: flex;
    padding: 30rpx;
    align-items: center;
    .pic_avatar {
      width: 100rpx;
      height: 100rpx;
      border: 1rpx solid #fff;
      border-radius: 50rpx;
      overflow: hidden;
    }
    .pic_user {
      margin-left: 30rpx;
      .pic_username {
        color: #000;
        font-size: 32rpx;
      }
      .pic_time {
        color: #ccc;
      }
    }
  }

  .pic_info {
    display: flex;
    border-bottom: 1rpx solid #eee;
    .pic_rank {
      flex: 1;
      display: flex;
      justify-content: center;
      padding: 20rpx 0;
      .pic_ranktext {
        margin-left: 20rpx;
      }
    }
    .pic_fav {
      flex: 1;
      display: flex;
      justify-content: center;
      padding: 20rpx 0;
      .pic_favtext {
        margin-left: 20rpx;
      }
    }
  }
}
</style>