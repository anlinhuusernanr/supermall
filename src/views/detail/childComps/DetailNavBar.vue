<template>
    <div>
        <nav-bar>
            <div slot="left">
                <img src="@/assets\img\common\back.svg" class="back" @click="backClick"/>
            </div>
            <div slot="center" class="title">
                <div v-for="(item,index) in title" class="title-item" 
                :class="{active : index === currentIndex}"
                @click="btnClick(index)">{{item}}</div>
            </div>
        </nav-bar>
    </div>
</template>

<script>
    import NavBar from "components/common/navbar/NavBar"

    export default {
        name: "DetailNavBar",
        data() {
            return {
                title: ['商品','参数','详情','评价'],
                currentIndex: 0
            }
        },
        components: {
            NavBar
        },
        methods: {
            btnClick(index) {
                this.currentIndex = index
            },
            backClick() {
                this.$router.back()
            }
        }
    }
</script>

<style>
    .active {
        color:var(--color-high-text);
    }
    .back {
        margin-top: 10px;
    }
    .title {
        display: flex;

    }
    .title-item {
        flex: 1;
        font-size: 13px;
    }
</style>