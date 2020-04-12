<template>
  <div>
    <UploadExcelComponent :on-success="handleSuccess" />
  </div>
</template>
<script>
import UploadExcelComponent from "@/components/HelloWorld.vue";

export default {
  name: "UploadExcel",
  components: { UploadExcelComponent },
  data() {
    return {
      userList: [],
      tableData: [],
      tableHeader: []
    };
  },
  methods: {
    beforeUpload(file) {
      const isLt1M = file.size / 1024 / 1024 < 1;
      if (isLt1M) {
        return true;
      }
      this.$message({
        message: "Please do not upload files larger than 1m in size.",
        type: "warning"
      });
      return false;
    },

    handleSuccess({ results, header }) {
      this.tableData = results;
      this.tableHeader = header;

      var user = {};
      results.forEach((item, index, array) => {
        //执行代码
        user.username = item.姓名;
        user.sex = item.性别;
        user.age = item.年龄;
        this.userList[index] = user;
        user = {};
      });

      console.log(this.userList);
      
      this.postRequest("/setUser", JSON.stringify(this.userList));//上传接口
    }
  }
};
</script>
<style scoped>
</style>
