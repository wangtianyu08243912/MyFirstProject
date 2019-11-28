<template>
    <div>
      <mt-header fixed title="企业详情">
        <router-link to="/enterpriseInformationEp" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>
      <div class="header">
        <ul>
          <li>企业名称<span>{{form.companyName}}</span></li>
          <li>乡镇 <span>{{form.townId}}</span></li>
          <li>行业<span>{{form.agriculturalClassification}}</span></li>
          <li>地址<span>{{form.companyAddress}}</span></li>
          <li>社会信用统一代码<span>{{form.creditCode}}</span></li>
          <li>联系人<span>{{form.contactPerson}}</span></li>
          <li>联系手机<span>{{form.contactMobile}}</span></li>
          <li>种植<span>水稻</span></li>
          <li>种植面积<span>{{form.productInfo.data_0_1}}</span></li>
        </ul>
        <div class="mui-card">
          <div class="mui-card-header">企业荣誉</div>
          <div class="mui-card-content">
            <div class="mui-card-content-inner">
              {{form.companyHonor}}
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
  import Request from "@/configs/request.js"
    export default {
        name: "CompanyDetailsEp", data(){
        return{
          form: {
            agriculturalClassification: 1,
            chargePerson: "",
            companyAddress: "",
            companyHonor: "",
            companyId: 0,
            companyName: "",
            companyType: "1",
            contactMobile: "",
            contactPerson: "",
            contactWay: "",
            createUserId: 0,
            creditCode: "",
            doSupervision: "",
            landSource: "",
            plantArea: 0,
            public_license: 0,
            public_punish: 0,
            qualityStandardId: 0,
            quality_standard: 0,
            remarks: "",
            townId: 1,
            updateUserId: 0,
            productInfo: {
              data_0_0: "",
              data_0_1: "",
              data_0_2: "",
              data_0_3: "",
              data_1_0: "",
              data_1_1: "",
              data_1_2: "",
              data_2_0: "",
              data_2_1: "",
              data_3_0: "",
              data_3_1: ""
            }
          },
        }
      },
      methods:{
        //企业详情
        getCompanyInfo(id) {
          console.log(id)
          let loader = this.$loading.show();
          Request()
            .get("/api/company_production/get/"+id)
            .then(response => {
              console.log(response)
              this.form = response;
              //将后台JSON字符串转为一个对象
              this.form.productInfo = JSON.parse(response.productInfo);
              setTimeout(() => {
                loader.hide();
              },500)
            })
            .catch(error => {
              console.log(error);
            });
        },

      },
      created() {
        console.log(this.$route.query.companyId);
        let id = this.$route.query.companyId;
        this.getCompanyInfo(id)
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
