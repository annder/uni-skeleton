<template>
  <view
    class="skeleton-container"
    :style="{
      height: windowWidth,
      width: windowHeight
    }"
  >
    <!-- 矩形骨头 -->
    <view
      class="skeleton-rect"
      v-for="(item, index) in rectList"
      :key="index"
      :style="[
        {
          height: `${item.height}px`,
          width: `${item.width}px`
        },
        {
          top: `${item.top}px`,
          bottom: `${item.bottom}px`,
          right: `${item.right}px`,
          left: `${item.left}px`
        }
      ]"
    ></view>
    <!-- 圆形骨头 -->
    <view
      class="skeleton-circular"
      v-for="(item, index) in circularList"
      :key="index"
      :style="[
        {
          height: `${item.height}px`,
          width: `${item.width}px`
        },
        {
          top: `${item.top}px`,
          bottom: `${item.bottom}px`,
          right: `${item.right}px`,
          left: `${item.left}px`
        }
      ]"
    ></view>
  </view>
</template>

<script>
export default {
  mounted() {
    const { windowWidth, windowHeight } = uni.getSystemInfoSync();
    if (windowWidth && windowHeight) {
      this.windowWidth = `${windowWidth}px`;
      this.windowHeight = `${windowHeight}px`;
    }
    this.drawRect();
    this.drawCircular();
  },

  data() {
    return {
      windowWidth: '',
      windowHeight: '',
      rectList: [],
      circularList: []
    };
  },
  methods: {
    /**
     * @description 选择所有元素
     * @param {String} selector
     * @return {Promise<object>}
     * */

    async selectAll(selector) {
      return new Promise((resolve, reject) => {
        uni
          .createSelectorQuery()
          .selectAll(selector)
          .boundingClientRect()
          .exec(res => resolve(res));
      });
    },
    // 绘制矩形框
    async drawRect() {
      const [rect] = await this.selectAll('.rect');
      this.rectList = rect;
    },
    // 绘制圆形框
    async drawCircular() {
      const [circular] = await this.selectAll('.circular');
      console.log('circular--->', circular);
      this.circularList = circular;
    }
  }
};
</script>

<style lang="scss">
$bg-color: #dcdde1;
// 骨总结构
.skeleton-container {
  position: absolute;
  left: 0;
  top: 0;
  z-index: 9999;
}
.skeleton-rect,
.skeleton-circular {
  position: absolute;
  background-color: $bg-color;
}
.skeleton-rect {
  border-radius: 4rpx;
}
.skeleton-circular {
  border-radius: 100%;
}
</style>
