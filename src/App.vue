<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/seller">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/goods">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue';
export default {
  data () {
    return {

    }
  },
  created () {
    this.$http.get('/api/seller'),then(response => {
      console.log(response)
    })
  },
  components: {
   'v-header': header
  }
};
</script>

<style lang="stylus" scoped>
  #app
    .tab
      display:flex;
      width:100%;
      height:40px;
      border-bottom:1px solid rgba(7, 17, 27, 0.1);
      .tab-item
        flex:1;
        & > a
          display:flex;
          width:100%;
          height:100%;
          justify-content:center;
          align-items:center;
          font-size:14px;
          color:rgb(77,85,93);
          text-decoration:none;
          &.active
            color:rgb(240,20,30);
</style>
