<template>
  <div>
    <mt-header fixed title="企业信息">
      <router-link to="/enterpriseHome" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    </mt-header>
    <div class="header">
      <div class="mui-input-row mui-search mui-active">
        <input type="search" class="mui-input-clear" placeholder="请输入产品名称" data-input-clear="1" data-input-search="1"><span class="mui-icon mui-icon-clear mui-hidden"></span><span class="mui-placeholder"><span class="mui-icon mui-icon-search"></span><span></span></span>
      </div>
      <div class="credit">
       <p>
         <select name="township" class="select" v-model="townId" @change="getList(1)">
           <option label="全部" :value="0"></option>
           <option v-for="item in TonwList" :key="item.id" :value="item.id" :label="item.name"></option>
         </select>
       </p>
        <p>
          <select name="industry" class="select" v-model="agriculturalClassification" @change="getList(1)">
            <option v-for="item in agricultural" :value="item.value" :label="item.label"></option>
          </select>
        </p>
      </div>
      <div class="page-infinite-list"
           v-infinite-scroll="loadMore"
           infinite-scroll-disabled="loading"
           infinite-scroll-distance="10">
      <ul v-for="item in tableData" @click="companyDetails(item.companyId)">
        <li>名称<span>{{item.companyName}}</span><span class="mui-icon mui-icon-arrowright"></span></li>
        <li>信用评级 <span>{{item.creditCode}}</span></li>
      </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import Request from "@/configs/request.js"
    export default {
        name: "EnterpriseInformationEp",
      data() {
        return {
          //农业分类
          agriculturalClassification: 0,
          //生产主体类型
          companyType:"",
          //乡镇
          townId: 0,
          page: {
            pageIndex: 0,
            pageSize: 20
          },
          agricultural: [
            {value: 0, label: '全部'},
            {value: 1, label: '养殖业'},
            {value: 2, label: '畜牧业'},
            {value: 3, label: '种植业'}
          ],
          //乡镇列表
          TonwList: [],
          //企业信息列表
          tableData: [],
          loading:false
        }
      },
      methods:{
        //进入企业详细页面
        companyDetails(id) {
          console.log(id);
          this.$router.push({path:'/companyDetailsEp',query:{ companyId: id }})
        },
        //获取乡镇列表
        getTonwList() {
          console.log(111);
          Request()
            .get("/api/town/all")
            .then(
              response => {
                console.log(response);
                this.TonwList = response;
              }
            )
        },
        //获取企业信息列表
        getList(type) {
          if(type==1){
            this.page.pageIndex=0;
          }
          this.loading = true ;
          this.tableData = [];
          let loader = this.$loading.show();
          Request()
            .get("/api/company_production/getAllList", {
              agriculturalClassification: this.agriculturalClassification,
              companyType: this.companyType,
              pageNo: this.page.pageIndex,
              pageSize: this.page.pageSize,
              townId: this.townId
            })
            .then(
              response => {
                console.log(response);
                this.page.pageIndex=this.page.pageIndex+1
                this.tableData = this.tableData.concat(response);
                if(response.length<this.page.pageSize){
                  this.loading = true ;
                }else{
                  this.loading = false ;
                }
                setTimeout(() => {
                  loader.hide();
                }, 500)
              }
            )
        },
        //上拉加载更多
        loadMore(){
          this.getList();
        }
      },
      created() {
        this.getTonwList();
      }
    }
</script>

<style scoped>
  .select{
    width: 80%;
    height: 100%;
  }
  .mui-input-clear{
    height: 3.3rem;
  }
  .credit{
    display: flex;
    height: 4rem;
    margin-top: -0.7rem;
  }
  .credit p{
    width: 50%;
    line-height: 3rem;
    text-align: left;
  }
  ul{
    border-bottom: 0.03rem black solid;
  }
  li{
    margin-bottom: 1rem;
    position: relative;
  }
  span{
    position: absolute;
    left: 9rem;
  }
  .mui-icon {
    margin-left: 50%;
    margin-top: 1rem
  }
</style>
