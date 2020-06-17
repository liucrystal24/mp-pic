<template>
  <view class="comments_list" v-if="comments.length!==0">
    <view class="comments_title">
      <text :class="iconstyle"></text>
      <text class="title_content">{{comment_title}}</text>
    </view>
    <view class="comment_item" v-for="item in comments" :key="item.id">
      <view class="comment_avatar">
        <image :src="item.user.avatar" mode="aspectFill" />
        <view class="comment_userinfo">
          <view class="comment_user">
            <view class="comment_username">{{item.user.name}}</view>
            <view class="comment_usertitle">
              <image v-for="item2 in item.user.title" :key="item2.name" :src="item2.icon" mode />
            </view>
          </view>
          <view class="comment_timedis">{{timedis(item.atime)}}</view>
        </view>
      </view>
      <view class="comment_content">
        <view class="content_info">{{item.content}}</view>
        <view class="content_fav">
          <text class="iconfont icon-dianzan">{{item.size}}</text>
        </view>
      </view>
    </view>
    <view class="comments_blank"></view>
  </view>
</template>
<script>
import moment from "moment";
moment.locale("zh-cn");
export default {
  data() {
    return {
      // iconstyle: ""
    };
  },
  mounted() {},
  methods: {
    timedis(atime) {
      return moment(atime * 1000).fromNow();
    }
  },
  computed: {
    iconstyle() {
      const title_icon =
        this.comment_title === "最热评论"
          ? "iconfont icon-github red"
          : "iconfont icon-weixin green";
      return title_icon;
    }
  },
  props: { comment_title: String, comments: Array }
};
</script>
<style lang='scss'>
.red {
  color: red;
  font-size: 40rpx;
}
.green {
  color: green;
  font-size: 40rpx;
}
.comments_list {
  .comments_title {
    padding: 20rpx 10rpx;
    .title_content {
      margin-left: 20rpx;
      font-size: 28rpx;
      color: #666;
      font-weight: 600;
    }
  }

  .comment_item {
    padding: 20rpx 10rpx;
    border-bottom: 10rpx solid #ddd;
    .comment_avatar {
      display: flex;
      image {
        width: 80rpx;
        height: 80rpx;
      }
      .comment_userinfo {
        padding-left: 20rpx;
        height: 80rpx;
        .comment_user {
          display: flex;
          justify-content: space-between;
          height: 40rpx;
          width: 640rpx;
          .comment_username {
            color: #333;
          }
          .comment_usertitle {
            display: flex;
            flex-wrap: nowrap;
            image {
              width: 40rpx;
              height: 40rpx;
            }
          }
        }

        .comment_timedis {
          height: 40rpx;
          color: #ddd;
        }
      }
    }

    .comment_content {
      margin-left: 110rpx;
      padding: 20rpx 0;
      display: flex;
      justify-content: space-between;
      color: #000;
      .content_info {
      }
      .content_fav {
      }
    }
  }

  .comments_blank {
    width: 750rpx;
    height: 20rpx;
    background-color: #ddd;
  }
}
</style>