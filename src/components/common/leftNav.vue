<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-goumai"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-shipin"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <div>统计</div>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <div>设置</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'leftNavs'
}
</script>

<style scoped>
.left-nav {
  float: left;
  width: 5%;
  height: 100%;
  color: #fff;
  background-color: #1d8ce0;
  font-size: 10px;
}

.iconfont {
  font-size: 24px;
}

.left-nav ul {
  padding: 0px;
  margin: 0px;
}

.left-nav li {
  list-style: none;
  text-align: center;
  border-bottom: 1px solid #20a0ff;
  padding: 10px;
}
</style>
