<template>
  <div id="app">
    <!-- 3 使用 -->
    <router-view/>
    <main-tab-barr/>
  </div>
</template>

<script>
// 1 导入
import MainTabBarr from './components/content/mainTabbar/MainTabBar'

export default {
  name: "App",
  // 2 注册
  components:{
    MainTabBarr
  }
};
</script>

<style>
/* 引入样式 */
@import 'assets/css/base.css';
</style>
