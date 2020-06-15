<template>
  <view>
    <!-- 作者、图片、点赞、收藏 -->
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
          <text class="iconfont icon-dianzan">{{imgCurrent.rank}}</text>
          <!-- <text class="pic_ranktext">{{imgCurrent.rank}}</text> -->
        </view>
        <view class="pic_fav">
          <text class="iconfont icon-shoucang1">收藏</text>
          <!-- <text class="pic_favtext">收藏</text> -->
        </view>
      </view>
    </view>
    <!-- 相关、专辑说明 -->
    <view class="pic_album_list">
      <view class="pic_album_title">相关</view>
      <view class="pic_album_item" v-for="item in album" :key="item.id">
        <image class="pic_album_cover" :src="item.cover" mode="aspectFill" />
        <view class="pic_album_info">
          <view class="pic_album_info_mark">专辑</view>
          <view class="pic_album_info_name">{{item.name}}</view>
          <view class="pic_album_arrow">
            <text class="iconfont icon-right"></text>
          </view>
        </view>
      </view>
    </view>
    <!-- 最热评论 -->
    <comment comment_title="最热评论" :comments="hot"></comment>
    <!-- 隔离带 -->
    <view class="comments_blank"></view>
    <!-- 最新评论 -->
    <comment comment_title="最新评论" :comments="comments"></comment>
  </view>
</template>
<script>
import moment from "moment";
import comment from "@/components/comments";
moment.locale("zh-cn");
export default {
  data() {
    return {
      // imgList: [],
      // imgIndex: 0,
      imgCurrent: {},
      imgsrc: "",
      dateDis: "",
      album: "",
      hot: "",
      comments: ""
    };
  },
  onLoad() {
    // this.imgList = getApp().globalData.imgList;
    // this.imgIndex = getApp().globalData.imgIndex;
    const { imgList, imgIndex } = getApp().globalData;
    this.imgCurrent = imgList[imgIndex];
    console.log(this.imgCurrent);
    // 修改图片格式
    // this.imgsrc =
    //   this.imgCurrent.thumb + this.imgCurrent.rule.replace("$<Height>", 360);
    this.imgsrc = this.imgCurrent.thumb;
    // 修改日期
    this.dateDis = moment(this.imgCurrent.atime * 1000).fromNow();

    // 获取图片id,并请求接口获取评论
    let { id } = this.imgCurrent;
    this.getComments(id);
  },
  methods: {
    getComments(id) {
      uni
        .request({
          url: `https://service.picasso.adesk.com/v2/wallpaper/wallpaper/${id}/comment`
        })
        .then(data => {
          let [err, res] = data;
          if (err === null) {
            console.log(res.data.res);

            // 赋值
            this.album = res.data.res.album;
            this.hot = res.data.res.hot;
            this.comments = res.data.res.comment;
          } else {
            console.log(err);
          }
        });
    }
  },
  components: { comment }
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
      text {
        font-size: 24rpx;
      }
    }
    .pic_fav {
      flex: 1;
      display: flex;
      justify-content: center;
      padding: 20rpx 0;
      text {
        font-size: 24rpx;
      }
    }
  }
}

.pic_album_list {
  padding: 10rpx;
  .pic_album_title {
    padding: 20rpx 0;
  }

  .pic_album_item {
    display: flex;
    position: relative;
    padding: 15rpx 0;
    border-bottom: 2rpx solid #eee;
    .pic_album_cover {
      width: 180rpx;
      height: 180rpx;
    }

    .pic_album_info {
      margin-left: 30rpx;
      .pic_album_info_mark {
        width: 100rpx;
        height: 50rpx;
        background-color: $color;
        color: #fff;
        display: flex;
        justify-content: center;
        align-items: center;
      }

      .pic_album_info_name {
        color: #000;
      }

      .pic_album_arrow {
        position: absolute;
        right: 100rpx;
        top: 50%;
        transform: translateY(-50%);
      }
    }
  }
}
.comments_blank {
  width: 750rpx;
  height: 20rpx;
  background-color: #ddd;
}
</style>