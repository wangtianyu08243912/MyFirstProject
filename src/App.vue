<template>
  <div id="app">
    <router-view :key="$route.fullPath + new Date()"></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {

}
.mint-search{
  height: 3.3rem;
}
.mint-header{
  height: 3rem;
}
  .header{
    margin-top: 3rem;
  }
  ul li{
    list-style-type:none;
  }
  /*顶部返回按钮*/
.mint-button-icon{
  margin-top: -0.1rem;
  }
</style>
