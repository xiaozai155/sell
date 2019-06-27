<template>
    <div class="seller" ref="box">
        <div class="seller-content" >
            <div class="seller-header">
                <div class="header-left">
                    <div class="header-left-a">{{seller.score}}</div>
                    <div class="header-left-b">综合评分</div>
                    <div class="header-left-c">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="header-right">
                    <div class="header-right-a">
                        <span class="header-right-a-a">服务态度</span>
                        <star class="header-right-a-b" :size='36' :score='seller.serviceScore'></star>
                        <span class="header-right-a-c">{{seller.serviceScore}}</span>
                    </div>
                    <div class="header-right-a">
                        <span class="header-right-a-a">服务态度</span>
                        <star class="header-right-a-b" :size='36' :score='seller.foodScore'></star>
                        <span class="header-right-a-c">{{seller.foodScore}}</span>
                    </div>
                    <div class="header-right-b"><span>送达时间</span><span class="header-right-b-a">{{seller.deliveryTime}}分钟</span></div>
                </div>
            </div>
            <star></star>
            <interval></interval>
                <div class="introduce">
                    <comment></comment>
                    <div class="evaluate">
                        <ul>
                            <li class="li" v-for="item in ratingsData" :key="item.id">
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
                    </div>
                </div>
        </div>
    </div>
</template>
<script>
import BScroll from 'better-scroll';
import star from '../star/star.vue';
import interval from '../interval/interval.vue';
import comment from '../comment/comment.vue';
const ALL = 2;
export default {
    props: {
        seller: {
            type: Object,
            onlyContent: true,
            selectType: ALL,
            desc: {
                all: '全部',
                positive: '满意',
                negative: '不满意'
            }
        }
    },
    data () {
        return {
            ratingsData: {}
        };
    },
    created () {
        this.$http.get('/api/ratings').then(response => {
            this.ratingsData = response.body.data;
            console.log(this.ratingsData);
        });
        this.showFlag = true;
        this.onlyContent = false;
        this.selectType = ALL;
        this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.box, {
                click: true
            });
        });
    },
    components: {
        star,
        interval,
        comment
    }
};
</script>
<style lang="stylus" scoped>
    .seller
        width: 100%
        position:absolute
        top: 184px
        left:0
        bottom:0
        overflow:hidden
        .seller-content
            width:100%
            .seller-header
                display: flex
                padding: 18px 0
                .header-left
                    padding: 0 22px
                    text-align:center
                    border-right: 1px solid #e6e7e8
                    .header-left-a
                        font-size: 24px
                        margin-top: 12px
                        color: rgb(255, 153, 0)
                        line-height: 28px
                    .header-left-b
                        font-size: 12px
                        color : rgb(7, 17, 27)
                        line-height: 12px
                        margin-top:6px
                    .header-left-c
                        font-size: 10px
                        color : #93999f
                        line-height: 10px
                        margin-top: 8px
                .header-right
                    padding: 0 0 0 24px
                    font-size: 6px
                    color: rgb(7, 17, 27)
                    .header-right-a
                        line-height:18px
                        margin-top: 8px
                        .header-right-a-a
                            vertical-align: top
                        .header-right-a-b
                            vertical-align: top
                            display:inline-block
                            margin-left: 6px
                        .header-right-a-c
                            vertical-align: top
                            margin-left: 6px
                            display:inline-block
                            color: #ff9900
                    .header-right-b
                        line-height:18px
                        margin-top: 8px
                    .header-right-b-a
                        margin-left: 12px
                        color: rgb(147, 153, 159)
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
