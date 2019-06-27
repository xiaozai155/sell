<template>
<div class="shopcart">
    <div class="content">
        <div class="shopcart-left" @click="toggleList">
            <div class="shop-left-a">
                <div class="icon" :class="{'color': calculateCunt > 0}">
                        <i class="icon-shopping_cart1" :class="{'color': calculateCunt > 0}"></i>
                </div>
                <div class="count" v-show="calculateCunt > 0">{{calculateCunt}}</div>
            </div>
            <div class="shop-left-b" :class="{'color': calculatePrice > 0}">￥{{calculatePrice}}</div>
            <div class="shop-left-c">另需配送费￥{{deliveryPrice}}</div>
        </div>
        <div class="shopcart-right" :class="{'color': calculatePrice >= minPrice}">{{present}}</div>
    </div>
    <transition name="slide-fade">
        <div class="particulars" v-show="listShow">
            <div class="header">
                <span class="header-a">购物车</span>
                <span class="header-b">清空</span>
            </div>
            <div class="content">
                <ul>
                    <li v-for="food in selectFoods" :key="food.id" class="content-a">
                        <div class="name">￥{{food.name}}</div>
                        <div class="price">
                            <span>{{food.price * food.count}}</span>
                            <addshop :foods = 'food'></addshop>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </transition>
</div>
 </template>
 <script>
 import addshop from '../addshop/addshop.vue';
 export default {
     data () {
         return {
             fold: false
         };
     },
     props: {
         selectFoods: {
             type: Array
         },
         deliveryPrice: {
             type: Number
         },
         minPrice: {
             type: Number
         }
     },
     computed: {
         calculatePrice() {
            let price = 0;
             this.selectFoods.forEach(item => {
                 price += item.price * item.count;
             });
             return price;
         },
         calculateCunt() {
             let cunt = 0;
             this.selectFoods.forEach(item => {
                 cunt += item.count;
             });
             return cunt;
         },
         present() {
             if ((this.calculatePrice < this.minPrice) && this.calculatePrice > 0) {
                 let item = this.minPrice - this.calculatePrice;
                 return `还差￥${item}元起送`;
             } else if (this.calculatePrice >= this.minPrice) {
                 return '去结算';
             } else {
                 return `￥${this.minPrice}起送`;
             }
         },
         listShow: {
             get: function() {
                 return this.fold;
             },
             set: function() {
                 if (!this.calculateCunt) {
                     this.fold = true;
                     return false;
                 }
                 let show = !this.fold;
                 return show;
             }
         }
     },
     methods: {
         toggleList() {
             if (!this.calculateCunt) {
                 return;
             }
             this.fold = !this.fold;
         }
     },
     components: {
          addshop
      }
 };
 </script>
 <style lang="stylus">
 .shopcart
     width:100%
     height:48px
     background:#131d26
     position:fixed
     left:0
     bottom:0
     z-index: 100
     .content
         display:flex
         height:48px
         .shopcart-left
            flex:1
            font-size: 0
            .shop-left-a
                vertical-align: top
                display: inline-block
                padding:6px
                width:52px
                height:52px
                box-sizing:border-box
                background:#131d26
                border-radius: 50%
                position:relative
                top:-10px
                left:18px
                z-index: 99
                .count
                    min-width: 20px
                    height: 20px
                    font-size: 12px
                    color : white
                    text-align: center
                    line-height: 20px
                    background: radial-gradient(red,white)
                    border-radius: 16px
                    position:absolute
                    top: 0
                    right: 0
                .icon
                    width:100%
                    height:100%
                    background: #2b343c
                    border-radius: 50%
                    line-height:40px
                    text-align: center
                    font-size: 24px
                    &.color
                        background: #00a0dc
                    i
                        color: #80858a
                        &.color
                            color: white
            .shop-left-b
                vertical-align: top
                display: inline-block
                font-size: 16px
                color: #919396
                font-weight: 700
                line-height:24px
                margin-top: 12px
                padding: 0 12px 0 18px
                border-right: 1px solid #2c343d
                &.color
                    color: white
            .shop-left-c
                display: inline-block
                vertical-align: top
                font-size: 12px
                color: #6a6e72
                line-height:24px
                margin-top: 12px
                padding: 0 12px 0 18px
        .shopcart-right
            font-size: 12px
            width:105px
            flex:0 0 105px
            background: #2b333b
            height: 100%
            line-height:48px
            text-align: center
            color: #979a9c
            font-weight: 400
            &.color
                background: #00b43c
                color: white
    .particulars
        position: absolute
        left: 0
        top: 0
        width: 100%
        transform: translateY(-100%)
        z-index:-1
        .header
            display: flex
            justify-content: space-between
            height:40px
            font-size: 14px
            background: #f3f5f7
            line-height: 40px
            text-align: center
            .header-a
                font-size: 14px
                color: rgb(7,17,27)
                margin-left:18px
            .header-b
                font-size: 14px
                color: rgb(0,160,220)
                margin-right:18px
        .content
            width:100%
            ul
                width:100%
            .content-a
                display: flex
                justify-content: space-between
                height 48px
                padding: 0 18px
                background: white
                line-height: 48px
                text-align: center
                .name
                    font-size:14px
                    color: rgb(7,17,27,0.1)
    .slide-fade-transition
        transition: all .6s ease
        transition: translate3d(0,-100%,0)
    .slide-fade-center, .slide-fade-leave
        transform: translate3d(0,0,0)
 </style>
