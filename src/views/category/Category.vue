<template>
  <div class="wrapper">
    <div class="content">
      <ul>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li><li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>

        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>

        <li>1</li>
        <li>1</li>
        <li>1</li>
      </ul>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll"

  export default {
    name: "Category",
    data() {
      return {
        sc:null
      }
    },
    mounted() {
     this.sc = new BScroll('.wrapper',{
      probeType: 2,
      pullUpLoad: true,
      click: true
      })
      this.sc.on('scroll',(option) => {
        // console.log(option)
      })
      this.sc.on('pullingUp',() => {
        console.log("上拉加载完成")
        this.sc.finishPullUp()
      })
    }
  }
  
</script>

<style scoped>
  .wrapper {
    height: 50vh;
    width: 100%;
    background-color: red;
    opacity: 0.5;
    color: black;
  }
</style>
