<template>
  <div>
    <mt-header fixed title="溯源详情">
      <router-link to="/home" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    </mt-header>
    <div class="header">
      <div class="mui-card">
        <div class="mui-card-header mui-header" style="position: relative">
          <div>寸草堂</div>
          <div style="position: absolute;left: 1rem;top: 3rem">溯源档案(编号:cct1190161666416)</div>
        </div>
        <div class="mui-card-content">
          <div class="mui-card-content-inner">
              <table>
                <tr>
                  <td>商品名称:</td>
                  <td>阳山水蜜桃</td>
                </tr>
                <tr>
                  <td>产地:</td>
                  <td>无锡阳山</td>
                </tr>
                <tr>
                  <td>品种:</td>
                  <td>2018早桃有机方式种植</td>
                </tr>
                <tr>
                  <td>采摘时间:</td>
                  <td>2019-03-19</td>
                </tr>
                <tr>
                  <td>种植园:</td>
                  <td>阳山镇寸草堂水蜜桃专业合作社</td>
                </tr>
                <tr>
                  <td>详细地址:</td>
                  <td>无锡市惠山区阳山镇光明村</td>
                </tr>
                <tr>
                  <td>园主:</td>
                  <td>尹文兰</td>
                </tr>
                <tr>
                  <td>批次:</td>
                  <td>cct1190161666416</td>
                </tr>
              </table>
          </div>
        </div>
      </div>

      <div class="mui-card">
        <div class="mui-card-header">
         作业:
        </div>
        <div class="mui-card-content">
            <span style="padding-left: 1rem">施肥 &nbsp&nbsp&nbsp&nbsp2017-10-10</span>
          <div class="mui-card-content-inner picter">
            图片
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Request from "@/configs/request.js"
  export default {
    name: "sweepCode",
    data(){
      return{

      }
    },
    methods:{
      //获取批次
      getBatch() {
        // let loader = this.$loading.show();
        Request()
          .get("/api/product_batch/all")
          .then(response => {
            console.log(response);
            // setTimeout(() => {
            //   loader.hide();
            // }, 0.5 * 1000);
          })
          .catch(error => {
            console.log(error);
          });
      },
      //获取溯源详情
      getData() {
        // let loader = this.$loading.show();
        Request()
          .get("/api/product_production/all")
          .then(response => {
            console.log(response);
            // setTimeout(() => {
            //   loader.hide();
            // }, 0.5 * 1000);
          })
          .catch(error => {
            console.log(error);
          });
      },
      getTracing_location() {
        // let loader = this.$loading.show();
        Request()
          .get("/api/tracing_location/all")
          .then(response => {
            console.log(response);
            // setTimeout(() => {
            //   loader.hide();
            // }, 0.5 * 1000);
          })
          .catch(error => {
            console.log(error);
          });
      },
    },
    created() {
      this.getData();
      this.getBatch();
      this.getTracing_location()
    }
  }
</script>

<style scoped>
  .header{
    padding-top: 0.5rem;
  }
  .mui-header{
    height: 4rem;
    margin-bottom: 0.5rem;
  }
  .mui-card-content{
    margin-top: 1rem;
  }
.picter{
  height: 10rem;
}
table{
  width: 100%;
  border-top:0.01rem #ccc solid;
  border-left:0.01rem #ccc solid ;
}
  tr{
    line-height: 2rem;
    border-bottom: 0.01rem #ccc solid;
  }
  td{
    text-align: center;
    border-right:0.01rem #ccc solid ;
  }
</style>
