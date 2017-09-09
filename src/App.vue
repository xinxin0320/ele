<template>
  <div id="app">
      <ele-header :seller="seller"></ele-header>
      <ul class="nav">
          <li><router-link to='/'>商品</router-link></li>
          <li><router-link to='/ratings'>评论</router-link></li>
          <li><router-link to='/seller'>商家</router-link></li>
      </ul>
      <router-view></router-view>
  </div>
</template>

<script>
//引入Header组件
import Header from './components/Header/Header'
//载入data.json
const shop = require('./api/data.json');
export default {
  name: 'app',
  data(){
      return {
          seller : {} //商家的信息，初始值为空
      }
  },
  created(){
      //通常，这里需要使用异步的方式向后台要数据，然后赋值给data
      this.seller = shop.seller;
  },
  components : {
      "ele-header" : Header //注册Header组件
  }
}
</script>

<style lang="less" scoped>
    .nav {
        display: flex;
        justify-content: space-around;
        height: 40px;
        line-height: 40px;
        border-bottom: 1px solid rgba(7,17,27,.1);

        a {
            color :rgb(77,85,93);
            font-size: 14px;
            text-decoration: none;
            &.router-link-exact-active{
                color:rgb(240,20,20);
            }
        }
    }
</style>
