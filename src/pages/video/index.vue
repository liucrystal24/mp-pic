<template>
  <view>
    <view class="tab_title">
      <view class="titleinner">
        <uni-segmented-control
          :current="current"
          :values="items.map((v) => v.title)"
          @clickItem="onClickItem"
          style-type="text"
          active-color="#d4237e"
        ></uni-segmented-control>
      </view>
      <view class="titlesearch">
        <text class="iconfont icon-sousuo"></text>
      </view>
    </view>
    <view class="title_content">
      <view v-if="current < 4">
        <video-wrap :request="{requesturl:items[current].request,keyword:items[current].keyword}"></video-wrap>
      </view>
      <view v-if="current === 4">
        <video-wrap :keyword="items[4].keyword" :request="items[4].request"></video-wrap>
      </view>
    </view>
  </view>
</template>
<script>
import videoWrap from "@/components/videoWrap";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  data() {
    return {
      items: [
        { title: "推荐", request: "/featured", keyword: "hot" },
        {
          title: "娱乐",
          request: "/category/59b25abbe7bce76bc834198a",
          keyword: "new"
        },
        { title: "最新", request: "/videowp", keyword: "new" }
        // { title: "热门", request: "/videowp", keyword: "hot" },
        // { title: "分类", request: "/videowp", keyword: "new" }
      ],
      current: 0
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  },
  components: { uniSegmentedControl, videoWrap }
};
</script>
<style lang='scss'>
.tab_title {
  position: relative;
  .titleinner {
    width: 60%;
    margin: 0 auto;
  }
  .titlesearch {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    right: 5%;
  }
}
</style>