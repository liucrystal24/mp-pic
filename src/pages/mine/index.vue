<template>
  <view class="content">
    <view class="user_info">
      <view class="user_title">
        <view class="iconfont icon-github user_avatar"></view>
        <view class="user_name">
          <view class="user_github">liucrystal24</view>
          <view class="user_wx">微信：Chrisliu24</view>
        </view>
      </view>
      <view class="user_friends">
        <view class="friends_items">
          <view class="friends_num">2</view>
          <view class="friends_title">主题</view>
        </view>
        <view class="friends_items">
          <view class="friends_num">30</view>
          <view class="friends_title">关注</view>
        </view>
        <view class="friends_items">
          <view class="friends_num">666</view>
          <view class="friends_title">粉丝</view>
        </view>
      </view>
    </view>
    <view class="user_tool">
      <view class="tool_item">
        <view class="tool_pic">
          <image src="../../static/mine/user_pic.png" mode="widthFix" />
        </view>
        <view class="tool_title">我的壁纸</view>
        <view class="iconfont icon-right tool_arrow"></view>
      </view>
      <view class="tool_item">
        <view class="tool_pic">
          <image src="../../static/mine/user_shopping.png" mode="widthFix" />
        </view>
        <view class="tool_title">金币商城</view>
        <view class="iconfont icon-right tool_arrow"></view>
      </view>
      <view class="tool_item">
        <view class="tool_pic">
          <image src="../../static/mine/user_advise.png" mode="widthFix" />
        </view>
        <view class="tool_title">意见反馈</view>
        <view class="iconfont icon-right tool_arrow"></view>
      </view>
      <view class="tool_item">
        <view class="tool_pic">
          <image src="../../static/mine/user_setting.png" mode="widthFix" />
        </view>
        <view class="tool_title">设置</view>
        <view class="iconfont icon-right tool_arrow"></view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: "我的"
    };
  },
  onLoad() {},
  methods: {}
};
</script>

<style lang='scss' scoped>
.content {
  // text-align: center;
  height: 100vh;
  background-color: #ddd;
  .user_info {
    // height: 200rpx;
    background-color: #fff;
    .user_title {
      display: flex;
      height: 200rpx;
      align-items: center;
      border-bottom: 1px solid #ddd;
      padding: 30rpx 30rpx;

      .user_avatar {
        font-size: 140rpx;
        width: 150rpx;
        height: 150rpx;
      }
      .user_name {
        margin-left: 55rpx;
        .user_github {
          font-size: 45rpx;
          font-weight: 600;
        }
        .user_wx {
          margin-top: 10rpx;
        }
      }
    }

    .user_friends {
      display: flex;
      text-align: center;
      padding: 30rpx 0;
      .friends_items {
        width: calc(100vw / 3);
        .friends_num {
          font-size: 30rpx;
        }
        .friends_title {
          font-size: 30rpx;
        }
      }
    }
  }

  .user_tool {
    margin-top: 20rpx;
    .tool_item {
      height: 130rpx;
      background-color: #fff;
      display: flex;
      align-items: center;
      padding: 10rpx 50rpx;
      border-bottom: 1rpx solid #ddd;
      .tool_pic {
        width: 70rpx;
      }
      .tool_title {
        margin-left: 30rpx;
        font-size: 30rpx;
      }
      .tool_arrow {
        position: absolute;
        right: 50rpx;
        color: #aaa;
      }
    }
  }
}
</style>
