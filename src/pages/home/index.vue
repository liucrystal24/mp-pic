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
      <view v-if="current === 0">
        <home-recommend></home-recommend>
      </view>
      <view v-if="current === 1">
        <home-category></home-category>
      </view>
      <view v-if="current === 2">
        <home-new></home-new>
      </view>
      <view v-if="current === 3">
        <home-album></home-album>
      </view>
    </view>
  </view>
</template>

<script>
import homeRecommend from "./home-recommend";
import homeCategory from "./home-category";
import homeNew from "./home-new";
import homeAlbum from "./home-album";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  data() {
    return {
      items: [
        { title: "推荐" },
        { title: "分类" },
        { title: "最新" },
        { title: "专辑" }
      ],
      current: 3
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  },
  onLoad() {
  },
  components: {
    homeRecommend,
    homeCategory,
    homeNew,
    homeAlbum,
    uniSegmentedControl
  }
};
</script>

<style lang="scss">
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
