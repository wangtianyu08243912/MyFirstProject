<template>
  <div>
    <mt-header fixed title="添加整改记录">
      <router-link to="/regulatoryRecordTow" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
    </mt-header>
    <div class="header">
      <ul>
        <li>日期<span>2019-08-12</span></li>
        <li>乡镇<span>梅里镇</span></li>
        <li>受检单位<span>江苏省无锡市欣欣农产品合作社</span></li>
        <li style="border: none">检查人<span>管霜怡，陈国庆</span></li>
      </ul>
      <div class="mui-card">
        <div class="mui-card-header">整改记录</div>
        <div class="mui-card-content">
          <div class="mui-card-content-inner">
            <table>
              <tr style="height: 30%;border-bottom: 0.01rem black solid; ">
                <td rowspan="2" style="border-right: 0.01rem black solid;width: 15%; text-align: center">结论</td>
                <td style="border-right: 0.01rem black solid;width: 15%;text-align: center">合格</td>
                <td style="padding-left: 1rem"><input type="radio" checked></td>
              </tr>
              <tr>
                <td style="border-right: 0.01rem black solid;width: 15%;text-align: center">不合格</td>
                <td style="padding-top: 1rem">
                  <mt-field label="责令修改"></mt-field>
                  <mt-field label="责令处罚"></mt-field>
                  <mt-field label="其他处理"></mt-field>
                </td>
              </tr>
            </table>
            <div class="scene">
              <span class="scenetitle">现场照片</span>
            </div>
            <div class="supervisor">
              <span>监管人签名</span>
            </div>
            <div class="confirming">
              <span>确认人签名</span>
            </div>
          </div>
        </div>
      </div>
      <mt-button size="large" type="primary" @click="">保存</mt-button>
    </div>
  </div>
</template>

<script>
  import Request from "@/configs/request.js"
  export default {
    name: "RectificationDetails",
    data() {
      return {
        images: "",
        fileName: "",
        signs: "",
        signName: "",
        confirmSigns: "",
        confirmSignName: "",
        superId: -1,
        listLoading: false,
        data: [],
        pageLoading: false,
        imageUrl_Live: "",
        imageUrl_Sign: "",
        imageUrl_ConfirmSign: "",
        file_live_1: null,
        file_live_2: null,
        file_live_3: null,
        township: [],
        companyList: [],
        conclusionData: null,
        isNanData: null,
        downloadURL: "",
        ruleFormValue: {
          townId: "",
          companyId: "",
          createTime: "",
          inspector: "",
          conclusion: "0",
          order: "",
          suggestion: "",
          others: ""
        },
        dialogVisible: false,
      };
    },
    methods:{
      //获取乡镇列表
      getTown() {
        Request()
          .get("/api/town/all")
          .then(response => {
            this.township = this.township.concat(response);
          })
          .catch(error => {
            console.log(error);
          });
      },
      //获取企业列表
      getCompanyProduct() {
        Request()
          .get("/api/company_production/name")
          .then(response => {
            this.companyList = this.companyList.concat(response);
          })
          .catch(error => {
            console.log(error);
          });
      },
      //获取整改记录详情
      getData() {
        console.log(this.superId);
        Request()
          .get(
            "/api/rectification_record/all?supervisionRecordId=" + this.superId,
            {
              sortBy: "id"
            }
          )
          .then(response => {
            console.log(response);
            let tmpdata = response.data;
            this.data = tmpdata[0];
            this.isNanData = this.data ? false : true;
            if (!this.isNanData)
              this.conclusionData = JSON.parse(this.data.conclusionFalseInfo);
          })
          .catch(error => {
            console.log(error);
          });
      },
      //
      onSubmit(formName) {
        var formData = new FormData();
        formData.append("scenePhotoFile", this.file_live_1); //required
        formData.append("signFile", this.file_live_2); //required
        formData.append("confirmor", this.file_live_3); //required
        formData.append("supervisionRecordId", this.superId); //required
        var newConclusionData;
        if (this.isNanData) {
          newConclusionData =
            this.ruleFormValue.conclusion == 1
              ? {
                order: " ",
                suggestion: " ",
                others: " "
              }
              : {
                order: this.ruleFormValue.order
                  ? this.ruleFormValue.order
                  : " ",
                suggestion: this.ruleFormValue.suggestion
                  ? this.ruleFormValue.suggestion
                  : " ",
                others: this.ruleFormValue.others
                  ? this.ruleFormValue.others
                  : " "
              };
          newConclusionData = JSON.stringify(newConclusionData);
          formData.append("id", 0); //required
          formData.append("towId", this.ruleFormValue.townId);
          formData.append("conclusion", this.ruleFormValue.conclusion);
          formData.append("inspector", this.ruleFormValue.inspector);
          formData.append("companyId", this.ruleFormValue.companyId);
          formData.append("updateTime", this.ruleFormValue.createTime);
          formData.append("updateUserId", Auth().user().id);
          formData.append("createUserId", Auth().user().id);
          formData.append("createTime", this.ruleFormValue.createTime);
          formData.append(
            "rectificationRecordTime",
            this.ruleFormValue.createTime
          );
          formData.append("conclusionFalseInfo", newConclusionData);
          this.$refs[formName].validate(valid => {
            if (valid) {
              this.listLoading = true;
              Request()
                .post("/api/rectification_record/create", formData)
                .then(response => {
                  setTimeout(() => {
                    this.listLoading = false;
                  }, 1000);
                  this.$router.push({ path: "/regulatoryRecord" });
                })
                .catch(error => {
                  console.log(error);
                });
            }
          });
        } else {
          newConclusionData = {
            order: this.conclusionData.order ? this.conclusionData.order : " ",
            suggestion: this.conclusionData.suggestion
              ? this.conclusionData.suggestion
              : " ",
            others: this.conclusionData.others ? this.conclusionData.others : " "
          };
          newConclusionData = JSON.stringify(newConclusionData);
          formData.append("townId", this.data.townId);
          formData.append("id", this.data.id); //required
          formData.append("inspector", this.data.inspector);
          formData.append("companyId", this.data.companyId);
          this.data.rectificationRecordTime = new Date(
            this.data.rectificationRecordTime
          ).toDateString("YYYY-MM-DD");
          formData.append("createTime", this.data.rectificationRecordTime);
          formData.append(
            "rectificationRecordTime",
            this.data.rectificationRecordTime
          );
          formData.append("updateUserId", Auth().user().id);
          formData.append("conclusionFalseInfo", newConclusionData);
          this.pageLoading = true;
          Request()
            .put("/api/rectification_record/update/" + this.data.id, formData)
            .then(response => {
              setTimeout(() => {
                this.pageLoading = false;
              }, 0.5 * 1000);
              this.$router.push({ path: "/regulatoryRecord" });
            })
            .catch(error => {});
        }
      },
      //下载
      downloadFile(imgURL) {
        axios({
          url: Urls.DOWNLOAD_URL() + imgURL,
          method: "GET",
          responseType: "blob" // important
        }).then(response => {
          const url = window.URL.createObjectURL(new Blob([response.data]));
          const link = document.createElement("a");
          link.href = url;
          link.setAttribute("download", imgURL.replace("/uploads/", "")); //or any other extension
          document.body.appendChild(link);
          link.click();
          link.remove();
        });
      }
    },
    created() {
      console.log(this.$route)
      this.superId = this.$route.query.id;
      this.getTown();
      this.getCompanyProduct();
      this.getData();
    }
  }
</script>

<style scoped>
  ul {
    margin-left: -2rem;
    font-weight: 800;
  }

  li {
    position: relative;
    line-height: 4rem;
    padding-left: 2rem;
    border-bottom: 0.03rem black solid;
  }

  span {
    position: absolute;
    left: 8rem;
    font-weight: 500;
  }

  table {
    width: 98%;
    height: 15rem;
    border: 0.02rem black solid;
  }

  .scene {
    height: 8rem;
    border: 0.01rem black solid;
    margin-top: 1rem;
    margin-bottom: 1rem;
    margin-right: 0.5rem;
  }

  .scene span {
    left: 3rem;
    line-height: 8rem;
  }

  .supervisor {
    height: 10rem;
    border: 0.01rem black solid;
    margin-bottom: 1rem;
    margin-right: 0.5rem;
  }

  .supervisor span {
    left: 3rem;
    line-height: 10rem;
  }

  .confirming {
    height: 10rem;
    border: 0.01rem black solid;
    margin-right: 0.5rem;
  }

  .confirming span {
    left: 3rem;
    line-height: 10rem;
  }
</style>
