<template>
  <div id="home">
    <!-- 导航 -->
    <nav-bar class="home-nav"><div slot="center">购物街</div> </nav-bar>
    <!-- 轮播图 -->
    <home-swiper :banners="banners" />
    <!-- 推荐 -->
    <recommends-view :recommends="recommends" />
    <!-- 本周流行 -->
    <feature-view />
    <!-- 选项卡 -->
    <tab-control
      class="tab-control"
      @tabClick="tabClick"
      :titles="['流行', '新款', '精选']"
    />
    <goods-list :goods="showGoods"></goods-list>
  </div>
</template>

<script>
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendsView from "./childComps/RecommendsView";
import FeatureView from "./childComps/FeatureView";

// 导入 导航公共组件
import NavBar from "@/components/common/navbar/Navbar";
// 选项卡公共组件
import TabControl from "@/components/content/tabControl/TabControl";
import goodsList from "@/components/content/goods/goodsList";

// 导入首页网络请求的数据
import { getHomeMultidata, getHomeGoods } from "@/network/home";

export default {
  components: {
    HomeSwiper,
    RecommendsView,
    FeatureView,
    NavBar,
    TabControl,
    goodsList,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
    };
  },
  // 组件创建完成 发送网络请求
  created() {
    // 尽量不要在created里写逻辑 直接写方法 简洁明了
    // 1 请求多个数据
    this.getHomeMultidata();

    // 2.请求商品数据
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  methods: {
    /* 
      网络请求相关
    */
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      // 动态获取商品页数
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list); //数据填进去 页数也要改
        this.goods[type].page += 1;
      });
    },
    /* 
      事件监听相关
    */
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
    },
  },
};
</script>

<style lang="css">
#home {
  padding-bottom: 50px;
}
.home-nav {
  background: var(--color-tint);
  color: #fff;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}
.tab-control {
  position: sticky;
  top: 44px;
}
</style>
