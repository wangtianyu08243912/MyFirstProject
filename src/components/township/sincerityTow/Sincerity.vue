<template>
    <div>
      <mt-header fixed title="诚信公示">
        <router-link to="/homeTow" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>
      <div class="header">
        <div class="mui-input-row mui-search mui-active">
          <input type="search" class="mui-input-clear" placeholder="请输入产品名称" data-input-clear="1" data-input-search="1"><span class="mui-icon mui-icon-clear mui-hidden"></span><span class="mui-placeholder"><span class="mui-icon mui-icon-search"></span><span></span></span>
        </div>
        <ul class="credit">
          <li @click="since(1)">A级(守信)</li>
          <li @click="since(2)">C级(失信)</li>
        </ul>
        <ul  v-for="row in List" :key="row.id">
          <li>名称<span>{{row.companyName}}</span></li>
          <li>社会统一信用代码 <span>{{row.creditCode}}</span></li>
          <li>信用评级 <span>{{row.Grade}}</span></li>
        </ul>
      </div>
    </div>
</template>

<script>
  import Request from "@/configs/request.js"
  export default {
    name: "Sincerity",
    data(){
      return{
        active:"tab-container1",
        List:[],
        bTypes: 0,
        currTown: 0,
        enterpriseName:""
      }
    },
    methods:{
      //诚信切换
      since(Type){
        this.bTypes=Type;
        this.getList();
      },
      //获取诚信公示列表
      getList(){
        let loader = this.$loading.show();
        Request()
          .get("/api/company_production/getCreditList",{
            companyType: this.bTypes,//公司类型
            townId: this.currTown //乡镇id
          })
          .then(
            response => {
              let list = response;
              list.forEach(function (item,i) {
                if(item.grade==null){
                  item.Grade="无评级"
                }
                else if(item.grade=="A"){
                  item.Grade="A级(守信)"
                }
                else if(item.grade=="B"){
                  item.Grade="B级(守信)"
                }
                else if(item.grade=="C"){
                  item.Grade="C级(失信)"
                }
              });
              this.List=list;
              console.log(this.List);
              setTimeout(() => {
                loader.hide();
              },500)
            }
          )
      }
    },
    created() {
      this.getList();
    }
  }
</script>

<style scoped>
  .mui-input-clear{
    height: 3rem;
  }
  .credit{
    border: none;
    display: flex;
    height: 3.5rem;
    background-color: #ccc;
    margin-top: -0.7rem;
  }
  .credit li{
    width: 50%;
    line-height: 3.5rem;
  }
  ul{
    border-bottom: 0.03rem black solid;
  }
  li{
    position: relative;
    margin-bottom: 1rem;
  }
span{
  position: absolute;
  left: 10rem;
}
</style>
