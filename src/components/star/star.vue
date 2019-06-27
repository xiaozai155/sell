<template>
    <div class="star" :class='starType'>
        <span v-for="item in itemClass" :class="item" class="star-item" :key="item.id">

        </span>
    </div>
</template>
<script>
    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_OFF = 'off';
    const CLS_HALF = 'half';
export default {
    props: {
        size: {
            tyle: Number
        },
        score: {
            tyle: Number
        }
    },
    computed: {
        starType() {
            return 'star-' + this.size;
        },
        itemClass() {
            let result = [];
            let score = Math.floor(this.score * 2) / 2;
            let hasDecimal = score % 1 !== 0;
            let integer = Math.floor(score);
            for (var i = 0; i < integer; i++) {
                result.push(CLS_ON);
            }
            if (hasDecimal) {
                result.push(CLS_HALF);
            }
            while (result.length < LENGTH) {
                result.push(CLS_OFF);
            }
            return result;
        }
    }
};
</script>
<style lang="stylus">
@import '../../common/stylus/mixin.styl';
        .star
            font-szie: 0
            .star-item
                display: inline-block
                background-repeat: no-repeat
        &.star-24
            .star-item
                width:10px
                height 10px
                background-size: 10px 10px
                margin-right: 5px
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')
        &.star-36
            .star-item
                width:15px
                height 15px
                background-size: 15px 15px
                margin-right: 5px
                &.on
                    bg-image('star36_on')
                &.off
                    bg-image('star36_off')
                &.half
                    bg-image('star36_half')
        &.star-48
            .star-item
                width:20px
                height 20px
                background-size: 20px 20px
                margin-right: 10px
                &.on
                    bg-image('star48_on')
                &.off
                    bg-image('star48_off')
                &.half
                    bg-image('star48_half')
</style>
