<template>
  <view>
    <view class="category_title">
      <view class="titleinner">
        <uni-segmented-control
          :current="current"
          :values="items.map((v) => v.title)"
          @clickItem="onClickItem"
          style-type="text"
          active-color="#d4237e"
        ></uni-segmented-control>
      </view>
    </view>
    <view class="category_list">
      <view v-if="current === 0">
        <category :cateid="cateid" :category="category"></category>
      </view>
      <view v-if="current === 1">
        <category :cateid="cateid" :category="category"></category>
      </view>
    </view>
  </view>
</template>
<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import category from "@/components/category";
export default {
  data() {
    return {
      items: [
        { title: "最新", category: "new" },
        { title: "热门", category: "hot" }
      ],
      current: 0,
      cateid: "",
      category: "new"
    };
  },
  onLoad(option) {
    this.cateid = option.id;
  },
  methods: {
    onClickItem(e) {
      this.category = this.items[e.currentIndex].category;
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  },
  components: { uniSegmentedControl, category }
};
</script>
<style lang='scss' scoped>
.category_title {
  position: relative;
  .titleinner {
    width: 60%;
    margin: 0 auto;
  }
}
</style>