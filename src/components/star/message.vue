<template>
    <div class="discounts">
        <div class="line"></div>
        <div class="d-item">{{item}}</div>
        <div class="line"></div>
    </div>
</template>
<script>
export default {
props: ['item']
};
</script>
<style lang="stylus" scoped>
.discounts
    display:flex
    width:80%
    margin:28px auto 24px auto
    .line
        flex:1
        border-bottom: 1px solid rgba(255,255,255,0.2)
        position:relative
        top:-6px
    .d-item
        font-size:14px
        color:rgb(255,255,255)
        margin: 0 12px
</style>
