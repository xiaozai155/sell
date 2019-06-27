<template>
    <div class="ratings" ref="con">
        <div class="ratings-content">
            <div class="title">
                <div class="title-a">
                    <div class="title-a-left">
                        <h3>{{seller.name}}</h3>
                        <div class="star-a"><star class="star" :size='36' :score='seller.deliveryPrice'></star><span>({{seller.ratingCount}})</span><span>月售{{seller.sellCount}}单</span></div>
                    </div>
                    <div class="title-a-right" @click="toggleFavorite">
                        <span class="icon-favorite1" :class="{'active': favorite}"></span>
                        <span class="a-span">{{favoriteText}}</span>
                    </div>
                </div>
                <div class="title-b">
                    <div class="title-b-a">
                        <span>起送价</span>
                        <div><i>{{seller.minPrice}}</i><sup>元</sup></div>
                    </div>
                    <div class="title-b-a">
                        <span>商家配送</span>
                        <div><i>{{seller.deliveryPrice}}</i><sup>元</sup></div>
                    </div>
                    <div class="title-b-a title-b-c">
                        <span>平均配送时间</span>
                        <div><i>{{seller.deliveryTime}}</i><sup>元</sup></div>
                    </div>
                </div>
            </div>
            <interval></interval>
            <div class="activity">
                <div class="notice">
                    <h2>公告与活动</h2>
                    <p>{{seller.bulletin}}</p>
                </div>
                <div class="discounts" v-for='(item,index) in seller.supports' :key= 'item.id'>
                    <span class="details-b" :class="classMap[seller.supports[index].type]"></span>
                    <span class="details-c">{{item.description}}</span>
                </div>
            </div>
            <interval></interval>
            <div class="action">
                <h2>商家实景</h2>
                <div class="action-a" ref="box">
                    <ul>
                        <li v-for="item in seller.pics" :key="item.id">
                            <img :src="item" alt="">
                        </li>
                    </ul>
                </div>
            </div>
            <interval></interval>
            <div class="message">
                <h2>商家信息</h2>
                <p v-for="item in seller.infos" :key="item.id">{{ item }}</p>
            </div>
        </div>
    </div>
</template>
<script>
import star from '../star/star.vue';
import interval from '../interval/interval.vue';
import BScroll from 'better-scroll';
export default {
    props: {
        seller: {
            type: Object
        }
    },
    data() {
        return {
            favorite: false
        };
    },
    created () {
        this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
        this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.con, {
                click: true
            });
            this.scrolls = new BScroll(this.$refs.box, {
                click: true,
                scrollX: true
            });
        });
    },
    methods: {
        toggleFavorite() {
            this.favorite = !this.favorite;
        }
    },
    computed: {
        favoriteText() {
            return this.favorite ? '已收藏' : '收藏';
        }
    },
    components: {
        star,
        interval
    }
};
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl';
    .ratings
        width: 100%
        position: absolute
        left:0
        top: 184px
        bottom: 0
        overflow: hidden
        .ratings-content
            .title
                padding:18px 18px 0 18px
                .title-a
                    display: flex
                    justify-content: space-between
                    border-bottom: 1px solid #e6e7e8
                    padding-bottom: 18px
                    .title-a-left
                        h3
                            font-size: 14px
                            color: rgb(7, 17, 27)
                            line-height: 14px
                        .star-a
                            margin-top: 8px
                            font-size: 10px
                            span
                                margin-left: 12px
                                display: inline-block
                                vertical-align: top
                                margin-top: 2px
                            .star
                                display:inline-block
                    .title-a-right
                        width: 36px
                        height: 39px
                        .icon-favorite1
                            display: block
                            font-size: 24px
                            color: #d4d6d9
                            line-height: 28px
                            text-align: center
                            &.active
                                color:rgb(240, 20, 20)
                        .a-span
                            display: block
                            font-size: 10px
                            color: rgb(77, 85, 93)
                            line-height: 10px
                            text-align: center
                .title-b
                    padding: 18px 0
                    display: flex
                    .title-b-a
                        flex : 1
                        text-align: center
                        border-right: 1px solid #e6e7e8
                        span
                            font-size: 10px
                            color: rgb(147, 153, 159)
                            line-height: 10px
                        div
                            i
                                font-size: 24px
                                font-weight: 200
                                color: rgb(7, 17, 27)
                                line-height: 24px
                            sup
                                font-size: 10px
                        &.title-b-c
                            border-right: none
            .activity
                padding: 18px
                .notice
                    border-1px(#eee)
                    padding-bottom: 16px
                    h2
                        font-size: 14px
                        color: rgb(7, 17, 27)
                        line-height: 14px
                        margin-bottom: 8px
                    p
                        font-size: 12px
                        font-weight: 200
                        color: rgb(240, 20, 20)
                        line-height: 24px
                        margin-left: 12px
                .discounts
                    padding:16px 0
                    border-1px(#eee)
                    .details-b
                        display:inline-block
                        width:16px
                        height:16px
                        background-size:16px 16px
                        line-height:16px
                        margin-left:12px
                        vertical-align : top
                        &.decrease
                            bg-image('decrease_2')
                        &.discount
                            bg-image('discount_2')
                        &.guarantee
                            bg-image('guarantee_2')
                        &.invoice
                            bg-image('invoice_2')
                        &.special
                            bg-image('special_2')
                    .details-c
                        font-size: 12px
                        font-weight: 200
                        color: rgb(7, 17, 27)
                        line-height: 16px
            .action
                padding: 18px
                h2
                    margin-bottom: 12px
                .action-a
                    overflow: hidden
                    ul
                        width:200%
                        display:flex
                        li
                            margin-left: 6px
                            img
                                width: 120px
                                height: 90px
            .message
                padding: 18px
                h2
                    border-1px(#eee)
                    padding-bottom: 12px
                p
                    font-size: 12px
                    font-weight: 200
                    color: rgb(7, 17, 27)
                    line-height: 16px
                    padding:16px 12px
                    border-1px(#eee)
</style>
