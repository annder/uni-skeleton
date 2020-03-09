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
        },
        {
          backgroundSize: `50vw ${item.height}px`
        }
      ]"
    ></view>
    <!-- 圆形骨头 -->
    <view
      class="skeleton-circular"
      v-for="(item, index) in items"
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
        },
        {
          backgroundSize: `50vw ${item.height}px`
        }
      ]"
    ></view>
  </view>
</template>

<script>
import { isString, isArray } from 'lodash';
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
      circular: []
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
      this.circularList = circular;
    }
  }
};
</script>

<style lang="scss">
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
}
// 矩形、圆形进度条
.skeleton-rect,
.skeleton-circular {
  background-color: #dcdde1; // 默认颜色
  background-image: linear-gradient(
    90deg,
    #dcdde1 0,
    #f5f6fa 50%,
    #dcdde1 100%
  );
  background-position: -150% 0;
  background-repeat: no-repeat;
  animation: loading 0.5s ease-in-out infinite;
}

.skeleton-rect {
  border-radius: 4rpx;
}
.skeleton-circular {
  border-radius: 100%;
}

@keyframes loading {
  to {
    background-position: 350% 0;
  }
}
</style>
