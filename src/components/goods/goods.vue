<template>
    <div class="goods">
        <div class="menu-wrapper" ref="menu">
            <ul>
                <li class="menu-a" v-for= '(item,index) in goods' :key="item.id" :class="{'conent':currentIndex == index}" @click='selectMenu($event,index)'>
                    <span class="item"><span class="icon" :class="classMap[item.type]" v-show= 'item.type > 0'></span>{{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref="foods">
            <ul>
                <li v-for="item in goods" :key="item.id" class="food-item food-item-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul class="food-content">
                        <li v-for='food in item.foods' :key="food.id" class="food-nav" @click="seleFood(food,$event)">
                            <div class="icon">
                                <img :src="food.icon" alt="" width='57' height='57'>
                            </div>
                            <div class="content">
                                <h2 class="content-title">{{food.name}}</h2>
                                <p>{{food.description}}</p>
                                <div class="extra">
                                    <span class="extra-a">月售{{food.sellCount}}份</span>
                                    <span class="extra-b">好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="price-a">{{food.price}}</span>
                                    <span class="price-b" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                    <div class="cunts">
                                        <addshop :foods = 'food' ></addshop>
                                    </div>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart :minPrice='seller.minPrice' :deliveryPrice='seller.deliveryPrice' :selectFoods='selectFoods'></shopcart>
        <food :food = 'selectFood' ref="food"></food>
    </div>
</template>
<script>
import addshop from '../addshop/addshop.vue';
import BScroll from 'better-scroll';
import shopcart from '../../components/shopcart/shopcart.vue';
import food from '../../components/food/food.vue';
const ERR_OK = 0;
export default {
    data() {
        return {
            goods: [],
            listHeight: [],
            scorll: 0,
            selectFood: {}
        };
    },
    props: {
        seller: {
            type: Object
        }
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scorll >= height1 && this.scorll < height2)) {
                    return i;
                };
            };
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach(item => {
                item.foods.forEach(food => {
                    if (food.count) {
                        foods.push(food);
                    };
                });
            });
            return foods;
        }
    },
    created() {
        this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
        this.$http.get('/api/goods').then(response => {
            response = response.body;
            if (response.errno === ERR_OK) {
                this.goods = response.data;
                this.$nextTick(() => {
                    this._linkScroll();
                    this._calculateHeight();
                });
            };
        });
    },
    methods: {
        seleFood (food, event) {
            if (!event._constructed) {
                return;
            };
            this.selectFood = food;
            this.$refs.food.show();
        },
        selectMenu(event, index) {
            if (!event._constructed) {
                return;
            };
            let foods = this.$refs.foods.getElementsByClassName('food-item-hook');
            let el = foods[index];
            this.foodScroll.scrollToElement(el, 300);
        },
        _linkScroll () {
            this.menuScroll = new BScroll(this.$refs.menu, {
                click: true
            });
            this.foodScroll = new BScroll(this.$refs.foods, {
                click: true,
                probeType: 3
            });
            this.foodScroll.on('scroll', (pos) => {
                let item = Math.abs(Math.round(pos.y));
                this.scorll = item;
            });
        },
        _calculateHeight() {
            let food = this.$refs.foods.getElementsByClassName('food-item-hook');
            let height = 0;
            this.listHeight.push(height);
            for (var i = 0; i < food.length; i++) {
                let item = food[i];
                height += item.clientHeight;
                this.listHeight.push(height);
            };
        }
    },
    components: {
        shopcart,
        addshop,
        food
    }
};
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl';
.goods
    display: flex
    position:absolute
    top:184px
    bottom:46px
    overflow:hidden
    width:100%
    .menu-wrapper
        flex:0 0 80px
        width:80px
        .menu-a
            display:table
            height:54px
            width:56px
            padding:0 12px
            background:#f3f5f7
            &.conent
                position:relative
                font-size:12px
                font-weight:700
                background:rgb(255,255,255)
                border-none()
                margin-top:-1px
                z-index:99
                .item
                    border-none()
            .item
                display:table-cell
                font-size:12px
                font-weight:200
                line-height:12px
                vertical-align :middle
                border-1px(rgba(7,17,27,0.1))
                // border-bottom:1px solid rgba(7,17,27,0.1)
                .icon
                    display:inline-block
                    width:12px
                    height:12px
                    background-size:12px 12px
                    &.decrease
                        bg-image('decrease_3')
                    &.discount
                        bg-image('discount_3')
                    &.guarantee
                        bg-image('guarantee_3')
                    &.invoice
                        bg-image('invoice_3')
                    &.special
                        bg-image('special_3')
    .foods-wrapper
        flex:1
        .food-item
            .title
                font size 12px
                color: rgb(147,153,159)
                height 26px
                border-left:4px solid #d9dde1
                line-height:26px
                padding-left:14px
                background: #f3f5f7
                line-height:26px
            .food-nav
                padding 18px
                display:flex
                border-1px(rgba(7,17,27,0.1))
                &:list-child
                    border-none()
                .icon
                    flex: 0 0 56px
                    width:56px
                .content
                    flex:1
                    margin-left:10px
                    .content-title
                        font size 14px
                        color: rgb(7,17,27)
                        line-height:14px
                        margin-top:2px
                    p
                        font-size: 10px
                        color: rgb(147,153,159)
                        line-height:10px
                        margin: 8px 0
                        width:100%
                        overflow:hidden
                        text-overflow:ellipsis
                    .extra
                        .extra-a,.extra-b
                            font-size: 10px
                            color: rgb(147,153,159)
                            line-height:10px
                    .price
                        position: relative
                        .price-a
                            font-szie:14px
                            color: rgb(240,20,20)
                            font-weight:700
                            line-height:24px
                        .price-b
                            font-szie:10px
                            text-decoration: line-through
                            color: rgb(147,153,159)
                            font-weight:700
                            line-height:24px
                        .cunts
                            position: absolute
                            right: 0
                            top:0

</style>
