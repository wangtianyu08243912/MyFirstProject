<template>
  <div>
    <mt-header fixed title="首页"></mt-header>
    <div class="header">乡镇</div>
    <ul>
      <li @click="regulatoryRecord"><img> 监管记录</li>
      <li @click="productionPtandard"><img>生产标准</li>
      <li @click="companyDetails"><img>企业信息</li>
      <li @click="sincerity"><img>诚信公示</li>
      <li><img>溯源扫码</li>
    </ul>
    <mt-tabbar>
      <mt-button size="large" type="primary" @click="exit">退出登录</mt-button>
    </mt-tabbar>
  </div>
</template>

<script>
  export default {
    name: "Home",
    methods: {
      exit() {
        this.$router.push('/')
      },
      //监管记录
      regulatoryRecord() {
        this.$router.push('/regulatoryRecordTow')
      },
      //生产标准
      productionPtandard() {
        this.$router.push('/productionPtandardTow')
      },
      //企业信息
      companyDetails() {
        this.$router.push('/enterpriseInformationTow')
      },
      //诚信公示
      sincerity() {
        this.$router.push('/sincerityTow')
      }
    }
  }
</script>

<style scoped>
  .header {
    margin-top: 3rem;
    height: 12rem;
    border: 0.1rem #ccc solid;
  }

  ul {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    margin-top: 4rem;
  }

  ul li {
    list-style-type: none;
    width: 33.33%;
    height: 6rem;
    margin-top: 1rem;
  }
</style>
