<template>
  <div id="home">
    <div class="home-nav">
      <nav-bar>
        <slot slot="center">购物车de guangli </slot>
      </nav-bar>
    </div>
    <tab-contorl :titles="['流行','最新','最热']" ref="tabcontorl1" @tabIndex="getIndex" v-show="isTabShow"></tab-contorl>

    <scroll class="content" ref="scroll" :probe-type="3" @scroll="contetnScroll" :pull-up-load="true"
      @loadMore="loadMore">
      <home-swiper :banners="banners" @imageLoad="imageLoad"></home-swiper>
      <home-recommend-view :recommends="recommends"></home-recommend-view>
      <feature-view></feature-view>
      <tab-contorl :titles="['流行','最新','最热']" ref="tabcontorl2" @tabIndex="getIndex"></tab-contorl>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backClick" v-show="isBackShow"></back-top>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar"
  import HomeSwiper from "./childCpms/HomeSwiper"
  import HomeRecommendView from "./childCpms/HomeRecommendView"
  import FeatureView from "./childCpms/FeatureView"
  import GoodsList from "components/content/goods/GoodsList"
  import Scroll from "components/common/scroll/Scroll"
  import BackTop from "components/content/backTop/BackTop"

  import TabContorl from "components/content/tabControl/TabContorl"

  import { getHomeMultidata, getHomeGoods } from "network/home"

  export default {
    name: "Home",
    data() {
      return {
        banners: [],
        recommends: [],
        Goods: {
          "pop": { page: 0, list: [] },
          "new": { page: 0, list: [] },
          "sell": { page: 0, list: [] },
        },
        currentType: 'pop',
        isBackShow: false,
        tabControlOffsetTop: 0,
        isTabShow: false,
        saveY: 0
      }
    },
    created() {
      //获取轮播数据
      this.getHomeMultidata()
      //获取首页商品数据
      this.getHomeGoods("pop")
      this.getHomeGoods("new")
      this.getHomeGoods("sell")
    },
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      this.$refs.scroll.refresh()
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()

    },
    mounted() {
      //监听图片加载完

      const refresh = this.debounce(this.$refs.scroll.refresh, 500)
      this.$bus.$on('itemImageLoad', () => {
        // console.log("=====")
        // this.$refs.scroll.refresh()
        refresh()

        //监听tabcontrolbar 

      })
    },
    components: {
      NavBar,
      HomeSwiper,
      HomeRecommendView,
      FeatureView,
      TabContorl,
      GoodsList,
      Scroll,
      BackTop,
    },
    computed: {
      showGoods() {
        return this.Goods[this.currentType].list
      }
    },
    methods: {
      imageLoad() {
        this.tabControlOffsetTop = this.$refs.tabcontorl2.$el.offsetTop

      },
      //防抖函数
      debounce(func, delay) {
        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },
      //事件监听
      getIndex(index) {
        //console.log(index)
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break;
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.tabcontorl1.currentIndex = index
        this.$refs.tabcontorl2.currentIndex = index
      },
      backClick() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      contetnScroll(position) {
        this.isBackShow = (-position.y) > 1000
        this.isTabShow = (-position.y) > this.tabControlOffsetTop
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
        //console.log("加载了")

        //this.$refs.scroll.scroll.refresh()
      },
      /*
      *网络请求相关
      */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type) {
        const page = this.Goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          // console.log(res)
          //this.Goods[type].page = res.data.page
          this.Goods[type].list.push(...res.data.list)
          this.Goods[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    /* padding-top: 44px; */
    height: 100vh;
    /* position: relative; */
  }

  .home-nav {
    background-color: var(--color-tint);
  }

  .tab-contorl {
    position: sticky;
    top: 44px;
    z-index: 9;
  }

  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
    /* height: 400px; */
  }
</style>