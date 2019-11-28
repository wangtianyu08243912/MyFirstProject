<template>
    <div>
      <mt-header fixed title="企业详情">
        <router-link to="/enterpriseInformationTow" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>
      <div class="header">
        <ul>
          <li>企业名称<span>稻米专业合作社</span></li>
          <li>乡镇 <span>梅里镇</span></li>
          <li>行业<span>种植业</span></li>
          <li>地址<span>常熟市梅李镇123号</span></li>
          <li>社会信用统一代码<span>12312312312313</span></li>
          <li>联系人<span>李三</span></li>
          <li>联系手机<span>15623644466</span></li>
          <li>种植<span>水稻</span></li>
          <li>种植面积<span>1200亩</span></li>
        </ul>
        <div class="mui-card">
          <div class="mui-card-header">企业荣誉</div>
          <div class="mui-card-content">
            <div class="mui-card-content-inner">
              农产品加工技术创新机构
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
  import apiConfig from "@/configs/api"
    export default {
        name: "CompanyDetails",
      methods:{
        //企业详情
        async getsupervisionRecordList() {
          let that = this;
          let response = await that.util.enterpriseinformationDetails(apiConfig.company_productionDetails, {});
          if (response.status==200){

          }
        },
      }
    }
</script>

<style scoped>
  ul{
    margin-top: 4rem;
    margin-left: -2rem;
  }
  li{
    position: relative;
    border-bottom: 0.02rem black solid;
    line-height: 3rem;
    padding-left: 1.5rem;
  }
  span{
    position: absolute;
    left: 12rem;
  }
  .mui-card-content-inner{
    height: 10rem;
  }
</style>
