<template>
  <div id="home" >
    <div class="home-nav">
      <nav-bar>
        <slot slot="center">购物车</slot>
      </nav-bar>
    </div>
    <home-swiper :banners="banners"></home-swiper>
    <home-recommend-view :recommends="recommends"></home-recommend-view>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar"
  import HomeSwiper from "./childCpms/HomeSwiper"
  import HomeRecommendView from "./childCpms/HomeRecommendView"

  import { getHomeMultidata } from "network/home"
 
  export default {
    name: "Home",
    data(){
      return {
        banners: [],
        recommends: []
      }
    },
    created() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
    components: {
      NavBar,
      HomeSwiper,
      HomeRecommendView
    },
    methods: {
     
    }
  }
</script>

<style scoped>
  .home-nav{
    background-color: var(--color-tint);
  }
</style>
