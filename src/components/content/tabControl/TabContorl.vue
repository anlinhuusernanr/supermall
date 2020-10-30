<template>
    <div class="tab-contorl">
        <div v-for="(item,index) in titles"
         class="tab-contorl-item"
         :class="{active: index==currentIndex}"
         @click="itemClick(index)">
            <span>{{item}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TabContorl",
        props: {
            titles: {
                type:Array,
                default() {
                    return []
                }
            }
        },
        data() {
            return {
                currentIndex: 0
            }
        },
        methods: {
            itemClick(index) {
                this.currentIndex = index
                this.$emit('tabIndex',index)
            }
        }
    }
</script>

<style>
    .tab-contorl {
        display: flex;
        height: 40px;
        line-height: 40px;
        font-size: 15px;
        text-align: center;
        background-color: #ffffff;
    }

    .tab-contorl-item {
        flex: 1;
    }

    .tab-contorl-item span {
        padding: 5px;
    }
    .active {
        color: var(--color-high-text);
    }

    .active span{
        border-bottom: 3px solid var(--color-high-text);
    }

</style>