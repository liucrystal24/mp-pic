<template>
  <scroll-view
    @scrolltolower="handleToLower"
    class="recommend_view"
    scroll-y
    v-if="recommendList.length > 0"
  >
    <!-- 推荐 -->
    <view class="recommend_wrap">
      <view class="recommend_title">
        <text class="title_content">推荐</text>
      </view>
      <view class="recommend_content">
        <view class="recommend_item" v-for="item in recommendList" :key="item.id">
          <!-- <navigator :url="`/pages/album/index?id=${item.target}`"> -->
          <image :src="item.thumb" mode="widthFix" />
          <!-- </navigator> -->
        </view>
      </view>
    </view>
    <!-- 月份 -->
    <view class="months_wrap">
      <view class="months_title">
        <view class="months_title_info">
          <view class="months_data">
            <text>{{ monthsList.MM }}</text>
            <text>/</text>
            <text>{{ monthsList.DD }}</text>
          </view>
          <view class="months_slogan">{{ monthsList.title }}</view>
        </view>
        <view class="months_title_more">更多 ></view>
      </view>
      <view class="months_content">
        <view class="months_item" v-for="(item,index) in monthsList.items" :key="item.id">
          <go-detail :list="monthsList.items" :index="index">
            <image :src="item.thumb + item.rule.replace('$<Height>', 360)" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 热门 -->
    <view class="hot_wrap">
      <view class="hot_title">
        <text class="title_content">热门</text>
      </view>
      <view class="hot_content">
        <view class="hot_item" v-for="(item,index) in hotList" :key="item.id">
          <go-detail :list="hotList" :index="index">
            <image :src="item.thumb" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
    </view>
  </scroll-view>
</template>
<script>
import moment from "moment";
import goDetail from "@/components/goDetail";
export default {
  data() {
    return {
      // 推荐列表
      recommendList: [],
      // 月份
      monthsList: {},
      // 热门
      hotList: [],
      // 请求参数
      params: {
        limit: 30,
        order: "hot",
        skip: 0
      },
      hasMore: true
    };
  },
  mounted() {
    this.getList();

    uni.setNavigationBarTitle({
      title: "推荐"
    });
  },
  methods: {
    // 触底处理
    handleToLower: function() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        uni.showToast({
          title: "没有数据了……",
          icon: "none"
        });
      }
    },
    // 获取接口数据
    getList: function() {
      uni.showLoading({
        title: "加载中"
      });
      uni
        .request({
          // url: "https://157.122.54.189:9088/image/v3/homepage/vertical",
          // 备用链接
          url: "https://service.picasso.adesk.com/v3/homepage/vertical",
          data: this.params
        })
        .then(data => {
          let [err, res] = data;
          uni.hideLoading();
          if (err === null) {
            console.log(res.data);
            // 第一次发请求
            if (this.recommendList.length === 0) {
              // 头部推荐模块
              this.recommendList = res.data.res.homepage[1].items;

              // 月份模块
              this.monthsList = res.data.res.homepage[2];
              this.monthsList.MM = moment(this.monthsList.stime).format("MM");
              this.monthsList.DD = moment(this.monthsList.stime).format("DD");
            }
            // 没有新数据时，弹窗提示
            if (res.data.res.vertical.length === 0) {
              this.hasMore = false;
              uni.showToast({
                title: "没有数据了",
                icon: "none"
              });
              return;
            }
            // 热门
            this.hotList = [...this.hotList, ...res.data.res.vertical];
          } else {
            console.log(err);
          }
        });
    }
  },
  components: { goDetail }
};
</script>
<style lang="scss" scoped>
.recommend_view {
  height: calc(100vh - 36px);
}
.recommend_wrap {
  .recommend_title {
    padding: 20rpx;
    text {
      border-left: 10rpx solid $color;
      padding-left: 20rpx;
      font-size: 34rpx;
      font-weight: 600;
    }
  }
  .recommend_content {
    display: flex;
    flex-wrap: wrap;
    .recommend_item {
      width: 50%;
      border: 5rpx solid #fff;
    }
  }
}

.months_wrap {
  .months_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .months_title_info {
      display: flex;
      font-size: 30rpx;
      .months_data {
        color: $color;
        font-size: 34rpx;
      }
      .months_slogan {
        margin-left: 30rpx;
        color: #666;
        font-weight: 600;
      }
    }
    .months_title_more {
      color: $color;
      font-size: 32rpx;
      font-weight: 400;
    }
  }

  .months_content {
    display: flex;
    flex-wrap: wrap;
    .months_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}

.hot_wrap {
  .hot_title {
    padding: 20rpx;
    text {
      border-left: 10rpx solid $color;
      padding-left: 20rpx;
      font-size: 34rpx;
      font-weight: 600;
    }
  }

  .hot_content {
    display: flex;
    flex-wrap: wrap;
    .hot_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}
</style>
