<template>
  <div class="left-nav">
      <ul>
          <li>
              <i class="icon iconfont icon-wodezichan"></i>
              <p>收银</p>
          </li>
           <li>
              <i class="icon iconfont icon-jinrudianpu"></i>
              <p>店铺</p>
          </li>
           <li>
              <i class="icon iconfont icon-liebiao"></i>
              <p>商品</p>
          </li>
           <li>
              <i class="icon iconfont icon-31huiyuanqia"></i>
              <p>会员</p>
          </li>
           <li>
              <i class="icon iconfont icon-31tuwenxiangqing"></i>
              <p>统计</p>
          </li>
           <li>
              <i class="icon iconfont icon-shezhi"></i>
              <p>设置</p>
          </li>
      </ul>
  </div>
</template>


<script>
export default {
  name: 'leftNav',
  data(){
      return{

      }
  }
}
</script>

<style scoped>
    .left-nav{
        width:5%;
        height: 100%;
        background: #1d8ce0;
        float: left;
        font-size:10px;
    }
    .iconfont{
        font-size:24px;
    }

    .left-nav ul{
        margin:0;
        padding:0;
    }
    .left-nav li{
        list-style: none;
        text-align: center;
        border-bottom: 1px solid lightgray;
        padding: 10px;
    }
</style>


