<template>
    <swiper>
        <swiper-item v-for="item in banners">
          <a :href="item.link">
            <img :src="item.image" @load="imageLoad">
          </a>
        </swiper-item>
      </swiper>
</template>

<script>
    import {Swiper, SwiperItem} from "components/common/swiper"

    export default{
        name: "HomeSwiper",
        props:{
            banners:{
                type:Array,
                default(){
                    return []
                }
            }
        },
        data() {
            return {
                isImageLoad: false
            }
        },
        components:{
            Swiper,
            SwiperItem
        },
        methods: {
            imageLoad() {
               if(!this.isImageLoad) {
                this.$emit("imageLoad")
                this.isImageLoad = true
               }
            }
        }
    }
</script>

<style>

</style>