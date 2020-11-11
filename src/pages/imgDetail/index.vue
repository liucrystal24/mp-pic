<template>
  <view>
    <!-- 作者、图片、点赞、收藏 -->
    <view class="pic_detail">
      <view class="pic_title">
        <view class="pic_avatar">
          <image :src="imgCurrent.user.avatar || userdefault.avatar" mode="widthFix" />
        </view>
        <view class="pic_user">
          <view class="pic_username">{{imgCurrent.user.name ||userdefault.name}}</view>
          <view class="pic_time">{{dateDis}}</view>
        </view>
      </view>
      <view class="pic">
        <touch-move @touchResult="handleTouch">
          <image :src="imgCurrent.thumb" mode="widthFix" />
        </touch-move>
      </view>
      <view class="pic_info">
        <view class="pic_rank">
          <text class="iconfont icon-dianzan">{{imgCurrent.rank}}</text>
          <!-- <text class="pic_ranktext">{{imgCurrent.rank}}</text> -->
        </view>
        <view class="pic_fav">
          <text class="iconfont icon-shoucang1">收藏</text>
        </view>
      </view>
    </view>
    <!-- 相关、专辑说明 -->
    <view class="pic_album_list" v-if="album.length !== 0">
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
    <!-- 最新评论 -->
    <comment comment_title="最新评论" :comments="comments"></comment>
    <view class="nocomment" v-if="hot.length === 0 && comments.length === 0">暂无评论……</view>
    <view class="download">
      <view class="downloadbutton" @click="handleDownload">下载图片</view>
    </view>
  </view>
</template>
<script>
import moment from "moment";
import comment from "@/components/comments";
import touchMove from "@/components/touchmove";
moment.locale("zh-cn");
export default {
  data() {
    return {
      imgList: [],
      imgIndex: 0,
      imgCurrent: {},
      imgsrc: "",
      dateDis: "",
      album: "",
      hot: "",
      comments: "",
      userdefault: {
        avatar:
          "https://wx.qlogo.cn/mmopen/PiajxSqBRaEJicbNovoeyfibbMDcrI7oXVcdDqf0nz3KnrL67LYLBHER8vQLs8A4nZkRSv494X0ekz5xMic8TwgphA/0",
        name: "神秘用户"
      }
    };
  },
  onLoad() {
    const { imgList, imgIndex } = getApp().globalData;
    this.imgIndex = imgIndex;
    this.imgList = imgList;
    this.imgCurrent = imgList[this.imgIndex];

    // 修改图片格式
    // this.imgsrc =
    //   this.imgCurrent.thumb + this.imgCurrent.rule.replace("$<Height>", 360);
    // this.imgsrc = this.imgCurrent.thumb;

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
    },
    handleTouch(data) {
      let currentIndex = this.imgIndex + data;
      if (currentIndex <= this.imgList.length - 1 && currentIndex >= 0) {
        this.imgIndex += data;
        this.imgCurrent = this.imgList[this.imgIndex];
        this.dateDis = moment(this.imgCurrent.atime * 1000).fromNow();
        let { id } = this.imgCurrent;
        this.getComments(id);
      } else {
        uni.showToast({
          title: "已经没有了...",
          icon: "none"
        });
      }
    },
    async handleDownload() {
      uni.showLoading({
        title: "下载中"
      });
      // console.log(this.imgCurrent.thumb);
      // this.imgCurrent.thumb 接口协议为 http，不满足微信发布，所以添加此图片
      let picurl =
        "https://b.zol-img.com.cn/sjbizhi/images/12/320x510/1604539833812.jpg";
      // 转换地址为 微信可下载的地址
      const [err, data] = await uni
        .downloadFile({
          url: picurl
        })
        .catch(err => {
          console.err(err);
        });
      console.log(data.tempFilePath);
      // 下载
      const result = await uni.saveImageToPhotosAlbum({
        filePath: data.tempFilePath
      });
      console.log(result);
      uni.hideLoading();
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

      console.log(result);
    }
  },
  components: { comment, touchMove }
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

.nocomment {
  padding: 20rpx;
}

.download {
  height: 120rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  .downloadbutton {
    width: 90%;
    background-color: $color;
    height: 85%;
    color: #fff;
    font-size: 50rpx;
    font-weight: 600;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>