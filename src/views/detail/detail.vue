<template>
    <div id="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
       <scroll class="content">
            <detial-swiper :topImages="topImages"></detial-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
       </scroll>
    </div>
</template>

<script>
    import DetailNavBar from "./childComps/DetailNavBar"
    import {getDetail,Goods,Shop} from "network/detail"
    import DetialSwiper from "./childComps/DetialSwiper"
    import DetailBaseInfo from "./childComps/DetailBaseInfo"
    import DetailShopInfo from "./childComps/DetailShopInfo"

     import Scroll from "@/components/common/scroll/Scroll"

    export default {
        name: "detail",
        data() {
            return {
                iid: null,
                topImages: [],
                goods: {},
                shop: {}
            }
        },
        components: {
            DetailNavBar,
            DetialSwiper,
            DetailBaseInfo,
            DetailShopInfo,
            Scroll
        },
        created() {
            this.iid = this.$route.params.iid
          //  console.log(this.$route.params.iid + "sad")

          getDetail(this.iid).then(res => {
              console.log(res)
              const data = res.result
              this.topImages = data.itemInfo.topImages
              //console.log(this.topImages)
              this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
              this.shop = new Shop(data.shopInfo)
          })
        

        }
    }
</script>

<style>
    #detail {
        color: #000000;
        position: relative;
        z-index: 9;
        background-color: #ffffff;
        height: 100vh;
    }
    .content {
        position: absolute;
        top: 0px;
        right: 0;
        bottom: 50px;
        left: 0;
        overflow: hidden;
        width: 100%;
        background-color: #ffffff;
            
    }
    .detail-nav {
        position: relative;
        z-index: 9;
        background-color: #ffffff;
    }
</style>