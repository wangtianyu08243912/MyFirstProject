<template>
    <div>
      <mt-header fixed title="投诉交流">
        <router-link to="/enterpriseHome" slot="left">
          <mt-button icon="back">返回</mt-button>
        </router-link>
      </mt-header>
      <div class="header">
         <div class="top">
           111
         </div>
        <div class="content">
            <h3>
              温馨提示
            </h3>
          <p>
            如有投诉交流，欢迎拨打12345平台热线!
          </p>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        name: "ComplaintCommunication"
    }
</script>

<style scoped>
  .top{
    height:12rem;
    border: 0.01rem black solid;
  }
  .content{
    width: 75%;
    margin: 0 auto;
    padding-top: 3rem;
  }
  .content h3{
    text-align: center;
  }
  .content p{
    text-align: center;
    margin-top: 1rem;
    font-size: 1rem;
    color: black;
  }
</style>
