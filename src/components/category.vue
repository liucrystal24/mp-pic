<template>
  <scroll-view scroll-y enable-flex class="category_new_list" @scrolltolower="handleToLower">
    <view class="category_new_item" v-for="(item,index) in vertical" :key="item.id">
      <go-detail :list="vertical" :index="index">
        <image :src="item.thumb" mode="widthFix" />
      </go-detail>
    </view>
  </scroll-view>
  <!-- </view> -->
</template>
<script>
import goDetail from "@/components/goDetail";
export default {
  data() {
    return {
      params: {
        limit: 12,
        skip: 0,
        order: this.category
      },
      vertical: [],
      hasMore: true
    };
  },
  mounted() {
    this.getList(this.cateid);
  },
  methods: {
    getList(id) {
      uni
        .request({
          url: `https://service.picasso.adesk.com/v1/vertical/category/${id}/vertical`,
          data: this.params
        })
        .then(data => {
          let [err, res] = data;
          if (err === null) {
            // console.log(res.data.res.vertical);
            console.log("测试res：" + res.data.res);
            if (res.data.res.vertical.length === 0) {
              this.hasMore = false;
              // 第一遍拉到底部是提示没有图片
              uni.showToast({
                title: "没有图片了",
                icon: "none"
              });
            } else {
              this.vertical = [...this.vertical, ...res.data.res.vertical];
            }
          }
        });
    },
    handleToLower() {
      if (this.hasMore) {
        this.params.skip += this.params.limit;
        this.getList(this.cateid);
      } else {
        // 数据全部加载完成，拉到底部提示没有图片
        uni.showToast({
          title: "没有图片了",
          icon: "none"
        });
      }
    }
  },
  props: {
    cateid: String,
    category: String
  },
  components: { goDetail }
};
</script>
<style lang='scss' scoped>
.category_new_list {
  height: calc(100vh - 36px);
  display: flex;
  flex-wrap: wrap;
  .category_new_item {
    width: 33.33%;
    border: 5rpx solid #fff;
    image {
      // width: 33.33%;
    }
  }
}
</style>