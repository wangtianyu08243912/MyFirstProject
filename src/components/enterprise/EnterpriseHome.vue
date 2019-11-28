<template>
    <div>
      <mt-header fixed title="首页"></mt-header>
      <div class="header">企业</div>

        <ul>
          <li @click="productionPtandard"><img>生产标准</li>
          <li @click="companyDetails"><img>企业信息</li>
          <li @click="sincerity"><img>诚信公示</li>
          <li><img>溯源扫码</li>
          <li @click="productionRecord"><img> 生产记录</li>
          <li @click="inputsUse"><img> 投入品使用</li>
          <li @click="complaintCommunication"><img> 投诉交流</li>
        </ul>
      <mt-tabbar>
        <mt-button  size="large" type="primary" @click="exit">退出登录</mt-button>
      </mt-tabbar>
    </div>
</template>

<script>
    export default {
        name: "EnterpriseHome",
      methods:{
        exit(){
          this.$router.push('/')
        },
        //生产标准
        productionPtandard(){
          this.$router.push('/productionPtandardEp')
        },
        //企业信息
        companyDetails(){
          this.$router.push('/enterpriseInformationEp')
        },
        //诚信公示
        sincerity(){
          this.$router.push('/sincerityEp')
        },
        //生产记录
        productionRecord(){
          this.$router.push('/productionRecord')
        },
        //投入品使用
        inputsUse(){
          this.$router.push('/inputsUse')
        },
        //投诉交流
        complaintCommunication(){
      this.$router.push('/complaintCommunication')
    },



      }
    }
</script>

<style scoped>
.header{
  margin-top: 3rem;
  height: 12rem;
  border: 0.1rem #ccc solid;
}
ul{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin-top: 4rem;
}
 ul li{
   list-style-type:none;
   width: 33.33%;
   height: 6rem;
   margin-top:1rem ;
  }
</style>
