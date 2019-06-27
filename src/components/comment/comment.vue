<template>
    <div class="comment">
        <div class="henter">
            <div :class="{active: selectType === 2}" class="block blue" @click="select(2)">{{desc.all}} <span>{{ratings.length}}</span></div>
            <div :class="{active: selectType === 0}" class="block blue" @click="select(0)">{{desc.positive}} <span>{{ positives.length }}</span></div>
            <div :class="{active: selectType === 1}" class="block gray" @click="select(1)">{{desc.negative}} <span>{{ negative.length }}</span></div>
        </div>
        <div class="content">
            <span @click="taggleContent" class="icon-check_circle1" :class="{active : onlyContent}"></span>
            <span class="content-a">只看有内容的评价</span>
        </div>
    </div>
</template>
<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
    props: {
        ratings: {
            type: Array,
            default () {
                return [];
            }
        },
        selectType: {
            type: Number,
            default () {
                return ALL;
            }
        },
        onlyContent: {
            type: Boolean,
            default () {
                return false;
            }
        },
        desc: {
            type: Object,
            default () {
                return {
                    all: '全部',
                    positive: '满意',
                    negative: '不满意'
                };
            }
        }
    },
    methods: {
        select (type) {
            this.selectType = type;
            this.$emit('ratingtype', type);
        },
        taggleContent () {
            this.onlyContent = !this.onlyContent;
            this.$emit('content', this.onlyContent);
        }
    },
    computed: {
        positives () {
            return this.ratings.filter((rating) => {
                return rating.rateType === POSITIVE;
            });
        },
        negative () {
            return this.ratings.filter((rating) => {
                return rating.rateType === NEGATIVE;
            });
        }
    }
};
</script>
<style lang="stylus">
    .comment
        .henter
            font-size:0
            margin: 0 18px
            padding: 18px 0
            border-bottom: 1px solid rgba(7,17,27,0.1)
            .block
                font-size: 12px
                color: rgb(77,85,93)
                line-height: 16px
                padding: 8px 12px
                display: inline-block
                margin-right:8px
                border-radius: 2px
                span
                    font-size: 8px
                    font-weight: 200
                &.blue
                    background: rgba(0,160,220,0.2)
                &.gray
                    background: rgba(77,85,93,0.2)
                &.active
                    background: rgb(0,160,220)
        .content
            font-size: 0
            line-height: 24px
            padding:12px 18px
            .icon-check_circle1
                display:inline-block
                font-size: 24px
                color: rgb(147,153,159)
                line-height: 48px
                &.active
                    color: #00b43c
            .content-a
                display:inline-block
                font-size: 12px
                color: rgb(147,153,159)
                margin-left: 4px
                vertical-align: top
                margin-top:12px
</style>
