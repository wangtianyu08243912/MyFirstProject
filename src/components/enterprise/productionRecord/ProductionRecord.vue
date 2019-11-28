<template>
    <div>
      <mt-header fixed title="生产记录">
        <router-link to="/enterpriseHome" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>
      <div class="header page-infinite-list"
           v-infinite-scroll="loadMore"
           infinite-scroll-disabled="loading"
           infinite-scroll-distance="10">
        <ul v-for="item in tableData" @click="go(item.id)">
          <li>产品<span>{{item.productName}}<a class="a">产量(公斤)</a>{{item.productionQuantity}}</span> </li>
          <li>产地<span>{{item.productionArea}}</span><span class="mui-icon mui-icon-arrowright"></span></li>
          <li>生产时长<span>{{item.productionTime}}天</span></li>
        </ul>
      </div>
    </div>
</template>

<script>
  import apiConfig from "@/configs/api"
  import Request from "@/configs/request.js"
    export default {
        name: "ProductionRecord",
      data(){
          return{
            page: {
              pageIndex: 0,
              pageSize: 20
            },
            total: 0,
            tableData: [],
            loading:false
          }
      },
      methods:{
        //跳转生产记录详情
        go(id){
          this.$router.push({path:'/productionRecordDetails',query:{
            id:id
          }})
        },
        //获取生产记录列表
        getData() {
          this.loading = true ;
          let loader = this.$loading.show();
          Request()
            .get("/api/production_record/all", {
              pageNo: this.page.pageIndex,
              pageSize: this.page.pageSize,
              sortBy: "id"
            })
            .then(response => {
              console.log(response);
              this.page.pageIndex=this.page.pageIndex+1
              this.tableData = this.tableData.concat(response.data);
              this.total = response.total;
              if(response.data.length<this.page.pageSize){
                this.loading = true ;
              }else{
                this.loading = false ;
              }
              setTimeout(() => {
                loader.hide();
              },500)
            })
            .catch(error => {
              console.log(error);
            });
        },
        //上拉加载更多
        loadMore(){
          this.getData();
        }
      },
      created() {

      }
    }
</script>

<style scoped>
  .header{
    padding-top: 1rem;
    margin-left: -1.2rem;
  }
  .a{
    text-decoration:none;
    color:#333;
    margin-left: 3rem;
    margin-right: 2rem;
    font-weight: 600;
  }
  ul{
    border-bottom: 0.03rem black solid;
  }
  li{
    position: relative;
    margin-bottom: 1rem;
    font-weight: 600;
  }
  span{
    position: absolute;
    left: 5.5rem;
    font-weight: 500;
  }
  .mui-icon{
    margin-left: 65%;
  }
</style>
