<template>
  <div id="app">
    <ele-header :seller="seller"></ele-header>
    <ul>
      <li><router-link to="/">商品</router-link></li>
      <li><router-link to="/ratings">评论</router-link></li>
      <li><router-link to="/seller">商家</router-link></li>
    </ul>
    <router-view></router-view>
  </div>
</template>

<script>
import Header from '@/components/Header/Header'
// import router from './router'
const shop=require('./api/data.json')

export default {
  name: 'app',
  // router,
  data(){
    return {
      seller:{}
    }
  },
  created(){
    this.seller=shop.seller;
  },
  components:{
    'ele-header':Header
  }
}
</script>

<style lang="less" scoped>
  ul{
    height:40px;
    line-height:40px;
    display:flex;
    justify-content:space-around;
    border-bottom:1px solid rgba(7,17,27,.1);
    a{
        font-size:14px;
        color:rgb(77,85,93);
        text-decoration:none;
        &.router-link-exact-active{
            color:rgb(240,20,20);
        }
    }
  }
</style>
