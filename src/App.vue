<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">商家</router-link>
      </div>
    </div>
    <router-view :seller = 'seller'></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue';
const ERR_OK = 0;
export default {
  data () {
    return {
      seller: {
        // id: (() => {
        //   let queryParam = urlParse();
        //   console.log(queryParam);
        //   return queryParam.id;
        // })
      }
    };
  },
  created () {
    this.$http.get('/api/seller').then(response => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = response.data;
      };
    });
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
