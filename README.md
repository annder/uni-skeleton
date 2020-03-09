# uni-app 骨架屏

## 安装

1. 将文件`src/Skeleton.vue`放入到 uni-app 的`components`目录下。

2. 注入到全局组件内

```diff
// main.js
import Vue from 'vue';
+ import Skeleton from "./components/Skeleton.vue"
import App from './App';

+ Vue.component('skeleton',Skeleton);

Vue.config.productionTip = false;
App.mpType = 'app';

const app = new Vue({
  store,
  ...App
});

app.$mount();
```

## 使用

```vue
<template>
  <view>
    <skeleton v-if="isLoading"></skeleton>
    <view class="block rect"></view>
  </view>
</template>

<script>
export default {
  onLoad() {
    let that = this;
    fetchData('your server address').then(res => {
      this.isLoading = false;
    });
  },
  data() {
    return {
      isLoading: true
    };
  }
};
</script>

<style>
.block {
}
</style>
```

## 选项

- `.rect` 矩形骨架
- `.circular` 圆形骨架


## 许可证

[MIT](https://opensource.org/licenses/MIT)