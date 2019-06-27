<template>
    <transition name="fade">
        <div class="food" v-show="showFlag" ref="foods">
            <div class="food-wrapper">
                <div class="food-content">
                    <div class="food-image">
                        <img :src="food.image" alt="">
                        <i class="icon-arrow_lift" @click="hidden"></i>
                    </div>
                    <div class="commodity">
                        <h2>{{ food.name }}</h2>
                        <div class="com-a">
                            <span class="com-a-a"> 月售{{ food.sellCount }}份</span><span class="com-a-b"> 好评率{{ food.rating }}%</span>
                        </div>
                        <div class="com-b">
                            <span class="com-b-a">￥{{ food.price }}</span><span class="com-b-b">{{ food.oldPrice }}</span>
                        </div>
                        <div class="join" @click="addFirst($event)" v-show="!food.count || food.count===0">
                            加入购物车
                        </div>
                        <div class="add">
                            <addshop :foods = 'food' v-show="food.count> 0" @ratingtype = 'select' @content = 'toggle'></addshop>
                        </div>
                    </div>
                </div>
                <interval></interval>
                <div class="details">
                    <h1>商品介绍</h1>
                    <p class="info">{{ food.info }}</p>
                </div>
                <interval></interval>
                <div class="introduce">
                    <h1>商品评价</h1>
                    <comment :only-content = 'onlyContent' :select-type = 'selectType' :desc = 'desc' :ratings = 'food.ratings'></comment>
                    <div class="evaluate">
                        <ul>
                            <li v-show="needShow(item.rateType,item.text)" class="li" v-for="item in food.ratings" :key="item.id">
                                <div class="eva-a">
                                    <div class="eva-a-a">
                                        <span>{{item.rateTime}}</span>
                                        <span></span>
                                    </div>
                                    <div class="eva-a-b">
                                        <span>{{item.username}}</span>
                                        <img :src="item.avatar" alt="">
                                    </div>
                                </div>
                                <div class="eva-b">
                                    <i :class="{ 'icon-thumb_up': item.rateType === 0,'icon-thumb_down': item.rateType === 1 }"></i>
                                    <span>{{ item.text }}</span>
                                </div>
                            </li>
                        </ul>
                        <div class="no-raing" v-show="!food.ratings || !food.ratings.length">
                            暂无评价
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>
<script>
import BScroll from 'better-scroll';
import addshop from '../addshop/addshop.vue';
import Vue from 'vue';
import interval from '../interval/interval.vue';
import comment from '../comment/comment.vue';
// const POSITIVE = 0;
// const NEGATIVE = 1;
const ALL = 2;
export default {
    data() {
        return {
            showFlag: false,
            onlyContent: true,
            selectType: ALL,
            desc: {
                all: '全部',
                positive: '推荐',
                negative: '吐槽'
            }
        };
    },
    props: {
        food: {
            type: Object
        }
    },
    methods: {
        show() {
            this.showFlag = true;
            this.onlyContent = false;
            this.selectType = ALL;
            this.$nextTick(() => {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.foods, {
                        click: true
                    });
                } else {
                    this.scroll.refresh();
                }
            });
        },
        hidden() {
            this.showFlag = false;
        },
        addFirst(event) {
            Vue.set(this.food, 'count', 1);
        },
        needShow(type, text) {
            if (this.onlyContent && !text) {
                return false;
            }
            if (this.selectType === ALL) {
                return true;
            } else {
                return type === this.selectType;
            }
        },
        select(type) {
            this.selectType = type;
        },
        toggle(onlyContent) {
            this.onlyContent = onlyContent;
        }
    },
    components: {
        addshop,
        interval,
        comment
    }
};
</script>
<style lang="stylus">
    .fade-enter-active, .fade-leave-active
        transition: all .5s ease
    .fade-enter, .fade-leave-to
        transform: translateX(100%)
    .food
        width: 100%
        z-index: 99
        position: fixed
        left: 0
        top: 0
        bottom: 48px
        background: white
        .food-content
            .food-image
                position:relative
                width:100%
                height:0
                padding-top:100%
                img
                    position:absolute
                    top:0
                    left:0
                    width:100%
                    height:100%
                i
                    font-size:18px
                    color:#fff
                    position:absolute
                    left:10px
                    top:10px
        .commodity
            padding: 15px
            position: relative
            h2
                font-size: 14px
                font-weight: 700
                color: rgb(7,17,27)
                line-height: 14px
            .com-a
                margin: 8px 0
                color: #d8dadc
                font-size: 10px
                .com-a-a
                    margin-right: 12px
            .com-b
                margin: 8px 0
                .com-b-a
                    font-size: 14px
                    color: #f24b4b
                    font-weight: 700
                    line-height: 28px
                .com-b-b
                    font-size: 10px
                    font-weight: 700
                    color: rgb(147,153,159)
                    line-height: 28px
                    text-decoration: none;
            .join
                position: absolute
                height 24px
                padding: 0 9px
                border-radius: 16px
                background: #00a0dc
                right: 16px
                bottom: 24px
                text-align: center
                line-height: 24px
                font-size:10px
                color: rgb(255,255,255)
            .add
                position: absolute
                right:16px
                bottom: 24px
    .details
        margin: 18px
        h1
            margin-bottom: 6px
        .info
            font-size: 12px
            font-weight: 200
            color: rgb(77, 85, 93)
            line-height: 24px
    .introduce
        border-bottom: 1px solid #e6e7e8
        h1
            margin: 18px 0 0 18px
        .evaluate
            padding: 0 18px
            .li
                border-bottom: 1px solid #e6e7e8
                padding: 16px 0
                .eva-a
                    display: flex
                    justify-content: space-between
                    .eva-a-a
                        font-size: 10px
                        color: rgb(147, 153, 159)
                        line-height: 12px
                    .eva-a-b
                         span
                            font-size: 10px
                            display: inline-block
                            color: rgb(147, 153, 159)
                            line-height: 12px
                         img
                            width: 12px
                            height: 12px
                            border-radius: 50%
                .eva-b
                    .icon-thumb_up
                        font-size: 12px
                        color: #00a0dc
                        line-height: 24px
                    .icon-thumb_down
                        font-size: 12px
                        color: rgb(147, 153, 159)
                        line-height: 24px
                    span
                        font-size: 12px
                        color: rgb(7, 17, 27)
                        line-height: 32px
</style>
