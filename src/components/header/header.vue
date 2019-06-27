<template>
    <div class="header">
        <div class="content-wrapper" @click="isFlag">
            <div class="avater">
                <img :src="seller.avatar" alt="">
            </div>
            <div class="content">
                <div class="title">
                    <span class="item"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
                <div class="su" v-if="seller.supports">
                    <span class="su-a" :class="classMap[seller.supports[0].type]"></span>
                    <span class="su-b">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div class="supports" v-if="seller.supports" >
                <span class="cunt">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right1"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" >
            <span class="bulletin"></span><span class='bulletin-c'>{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right1"></i>
        </div>
        <div class="posi">
            <img :src="seller.avatar" alt="">
        </div>
        <transition name="fade">
            <div class="backbround-wrapper" v-show="flag" transition='fade'>
                <div class="background clearfix">
                    <div class="background-b">
                        <h1 class="name">{{seller.name}}</h1>
                        <div class="satis">
                            <star :size='48' :score='seller.score'></star>
                        </div>
                        <message :item="mes"></message>
                        <div class="details">
                            <div class="details-a" v-for="(item,index) in seller.supports" :key="item.id">
                                <span class="details-b" :class="classMap[seller.supports[index].type]"></span>
                                <span class="details-c" >{{item.description}}</span>
                            </div>
                        </div>
                        <message :item="msg"></message>
                        <div class="details-item">
                            <p>{{seller.bulletin}}</p>
                        </div>
                    </div>
                </div>
                <div class="background-a">
                    <i class="icon-close1" @click='isfalse'></i>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
import star from '../star/star.vue';
import message from '../star/message.vue';
export default {
    data() {
        return {
            flag: false,
            mes: '优惠信息',
            msg: '商家公告'
        };
    },
    methods: {
        isFlag() {
            this.flag = true;
        },
        isfalse() {
            this.flag = false;
        }
    },
    props: {
        seller: {
            type: Object
        }
    },
    created() {
        this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
    },
    components: {
        star,
        message
    }
};
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl';
.header
    position: relative
    background: rgba(7,17,27,0.5)
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s;
        }
        .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0;
        }
    .content-wrapper
        position: relative
        display:flex
        padding:24px
        .avater
            img
                width:64px
                height 64px
        .content
            margin-top:2px
            margin-left:16px
            .title
                .item
                    vertical-align: top
                    display:inline-block
                    width:32px
                    height:18px
                    bg-image('brand')
                    background-size :32px 18px
                    background-repeat : no-repeat
                .name
                    font-size: 16px
                    color: rgb(255,255,255)
                    font-weight : bold
                    line-height:18px
            .description
                margin-top:4px
                font-size: 12px
                color: rgb(255,255,255)
                font-weight : 100
                line-height:20px
            .su
                margin-top:4px
                .su-a
                    width:12px
                    height:12px
                    display:inline-block
                    vertical-align : top
                    background-size : 12px 12px
                    &.decrease
                        bg-image('decrease_1')
                    &.discount
                        bg-image('discount_1')
                    &.guarantee
                        bg-image('guarantee_1')
                    &.invoice
                        bg-image('invoice_1')
                    &.special
                        bg-image('special_1')
                .su-b
                    font-size: 10px
                    color: rgb(255,255,255)
                    vertical-align : top
        .supports
            width: 48px
            height: 24px
            position:absolute
            right:12px
            bottom:24px
            background:rgba(0,0,0,0.2)
            border-radius: 14px
            color:rgb(255,255,255)
            line-height:24px
            font-size:10px
            text-align:center
            display:flex
            justify-content: center
            align-items: center
            i
                line-height : 24px
                font-size: 14px
    .bulletin-wrapper
        position: relative
        width:100%
        height 28px
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
        color: rgb(255,255,255)
        background: rgba(7,17,27,0.2)
        .bulletin
            display: inline-block
            width:22px
            height:12px
            bg-image('bulletin')
            background-size: 22px 12px
            background-repeat : no-repeat
            vertical-align :bottom
            margin:8px 4px 0 4px
        .bulletin-c
            font-size: 10px
            vertical-align :bottom
        .icon-keyboard_arrow_right1
            position: absolute
            right: 0
            bottom: 6px
    .posi
        width:100%
        height:100%
        position :absolute
        left:0
        top:0
        z-index:-1
        img
            width:100%
            height:100%
    .backbround-wrapper
        background: rgba(7,7,27,0.8)
        backdrop-filter: blur(10px)
        width:100%
        height:100%
        position:fixed
        top:0
        left:0
        overflow: auto
        color:rgb(255,255,255)
        z-index:100
        .background
           min-height:100%
           width:100%
           .background-b
                margin-top:64px
                padding-bottom: 64px
                width:100%;
                .name
                    font-size:16px
                    font-weight: 700
                    line-height: 16px
                    text-align:center
                .satis
                    width:100%
                    text-align:center
                    margin-top:16px
                .details
                    width:80%
                    margin: 0 auto 28px auto
                    .details-a
                        font-size:12px
                        color:rgb(255,255,255)
                        .details-b
                            display:inline-block
                            width:16px
                            height:16px
                            background-size:16px 16px
                            line-height:16px
                            margin-bottom:12px
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
                            display:inline-block
                            margin-top:2px
                .details-item
                    width:80%
                    margin:0 auto
                    font-size:12px
                    line-height:24px
        .background-a
            // position:relative
            width:32px
            height:32px
            margin:-64px auto 0 auto
            font-size:32px
            clear:both
</style>
