<template>
  <div>
    <mt-header fixed title="生产标准">
      <router-link to="/home" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    </mt-header>
    <div class="header">
      <div class="mui-input-row mui-search mui-active">
        <input type="search" class="mui-input-clear" placeholder="请输入标准名称" data-input-clear="1"
               data-input-search="1" v-model="productName"  v-on:change="getProductionStandardList()">
        <span class="mui-icon mui-icon-clear mui-hidden"></span><span class="mui-placeholder">
        <span class="mui-icon mui-icon-search"></span>
        <span></span></span>
      </div>
        <select name="industry" id="select" v-model="category" v-on:change="getProductionStandardList()">
          <option v-for="item in categoryOption" :key="item.value" :value="item.value">{{item.name}}</option>
        </select>
      <ul class="credit">
        <li>产品</li>
      </ul>

      <ul v-if="data.length">
        <li v-for="row in data" :key="row.id" @click="standardDetails(row.id)">{{row.productName}}<span class="mui-icon mui-icon-arrowright"
                                               style="margin-left: 8rem;margin-top: 1.6rem"></span></li>
      </ul>

      <div class="no-data" v-else>
        <p>没有数据</p>
      </div>
    </div>
  </div>
</template>

<script>
  import Request from "@/configs/request.js"
  export default {
    name: "ProductionPtandard",
    data() {
      return{
        data:[],
        productName:"",
        categoryOption : [
           {name:"全部", value:"0"},
           {name:"种植业", value:"1"},
           {name:"养殖业", value:"2"},
           {name:"畜牧业", value:"3"}
        ],
        category:'0'
      };
    },
    methods: {

      standardDetails(selectedId) {
        this.$router.push({path:'/StandardDetails',query:{
          selectedId : selectedId
        }})
      },
      
      //获取生产标准列表
      getProductionStandardList() {
        // let that = this;
        // let response = await that.util.productionStandard(apiConfig.production_standard, {});
        // console.log(response);
        let loader = this.$loading.show();
         Request()
        .get("/api/production_standard/all", {
          category: this.category,
          productName: this.productName,
          sortBy: "id"
        })
        .then(response => {
          this.data = response.data;
            setTimeout(() => {
              loader.hide();
            },500)    
        })
        .catch(error => {
          console.log(error);
        });
      },
    },
    created: function () {
      let that = this;
      that.getProductionStandardList();
    }
  }
</script>

<style scoped>
  .mui-input-clear {
    height: 3rem;
  }

  #select {
    width: 75%;
    border: 0.01rem black solid;
    margin-top: -0.7rem;
  }

  option {
    width: 10rem;
  }

  .credit {
    border: none;
    display: flex;
    height: 3rem;
    background-color: #ccc;
    margin-left: 0;
    margin-top: -0.7rem;
  }

  .credit li {
    width: 50%;
    line-height: 3rem;
    text-align: left;
    border: none;
    padding-left: 0;
  }

  ul {
    margin-left: -2rem;
  }
  .no-data {
    text-align: center;
  }
  li {
    position: relative;
    margin-bottom: 1rem;
    line-height: 5rem;
    padding-left: 2rem;
    border-bottom: 0.03rem black solid;
  }

  span {
    position: absolute;
    left: 12rem;
  }

  .right {
    margin-left: 8rem;
    margin-top: 1.6rem
  }
</style>
