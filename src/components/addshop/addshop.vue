<template>
   <div class="addshop" @click.stop = ''>
        <transition name="slide-fade">
            <div class="addshop-a icon-remove_circle_outline" v-show="foods.count > 0" @click="remove">
            </div>
        </transition>
        <div class="addshop-b" v-show="foods.count > 0">{{foods.count}}</div>
        <div class="addshop-c icon-add_circle" @click="addCart"></div>
   </div>
</template>
<script>
import Vue from 'vue';
export default {
    props: {
        foods: {
            type: Object
        }
    },
    methods: {
        addCart() {
            if (!this.foods.count) {
                Vue.set(this.foods, 'count', 1);
            } else {
                this.foods.count++;
            };
        },
        remove() {
            if (this.foods.count) {
                this.foods.count--;
            };
        }
    }
};
</script>
<style lang="stylus">
.addshop
    display:inline-block
    z-index:88
    .slide-fade-enter-active
        transition: all .3s ease
    .slide-fade-leave-active
        transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0)
    .slide-fade-enter, .slide-fade-leave-to
        transform: rotate(180deg)
        transform: translateX(10px)
        opacity: 0
    .addshop-a
        display: inline-block
        font-size: 24px
        color: #00a0dc
    .addshop-b
        display: inline-block
        vertical-align: top
        text-align: center
        line-height: 25px
    .addshop-c
        display: inline-block
        font-size: 24px
        color: #00a0dc
</style>
