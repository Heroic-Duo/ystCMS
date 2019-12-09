<template>
  <div class="dr-toolbar">
    <el-col :xs="12" :md="18" class="option-button">
      <el-button size="small" type="primary" plain round @click="addTag">
        <svg-icon icon-class="icon_add" />
      </el-button>
      <el-button size="small" type="danger" plain round @click="branchDelete('contentTag')">
        <svg-icon icon-class="icon_delete" />
      </el-button>
      <!-- TOPBARLEFT -->
    </el-col>
    <el-col :xs="12" :md="6">
      <div class="dr-toolbar-right">
        <el-input
          class="dr-searchInput"
          size="small"
          :placeholder="$t('topBar.tagName')"
          v-model="pageInfo.searchkey"
          suffix-icon="el-icon-search"
          @keyup.enter.native="searchResult"
        ></el-input>
      </div>
    </el-col>
  </div>
</template>
<script>
import { bakUpCMSData,deleteContentTag } from "@/api/contentTag";
export default {
  props: {
    device: String,
    pageInfo: Object,
    type: String,
    ids: Array
  },
  data() {
    return {
      selectUserList: [],
      searchkey: ""
    };
  },
  methods: {
    addTag() {
      this.$store.dispatch("contentTag/showContentTagForm");
    },
    branchDelete(target) {
     
      // let _this = this;
      // console.log(_this,'_this_this_this')
      // console.log(_.isEmpty,'_.isEmpty_.isEmpty_.isEmpty')
      // if (_.isEmpty(_this.ids)) {
      //   this.$message({
      //     type: "info",
      //     message: this.$t("validate.selectNull", {
      //       label: this.$t("main.target_Item")
      //     })
      //   });
      //   return false;
      // }
      this.$confirm(
        this.$t("main.del_notice"),
        this.$t("main.scr_modal_title"),
        {
          confirmButtonText: this.$t("main.confirmBtnText"),
          cancelButtonText: this.$t("main.cancelBtnText"),
          type: "warning"
        }
      )
        .then(() => {
          let ids = _this.ids.join();
          return deleteContentTag({
            ids
          });
        })
        .then(result => {
          if (result.status === 200) {
            this.$store.dispatch("contentTag/getContentList");
            this.$message({
              message: `${this.$t("main.scr_modal_del_succes_info")}`,
              type: "success"
            });
          } else {
            this.$message.error(result.message);
          }
        })
        .catch(err => {
          this.$message({
            type: "info",
            message: this.$t("main.scr_modal_del_error_info")
          });
        });
    },
    searchResult(ev) {
      let searchkey = this.pageInfo ? this.pageInfo.searchkey : "";
      this.$store.dispatch("contentTag/getContentTagList", {
        searchkey
      });
    }
    // TOPBARLEFTOPTION
  },
  components: {}
};
</script>
<style lang="scss">
</style>
