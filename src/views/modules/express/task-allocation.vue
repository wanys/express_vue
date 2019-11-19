<template>
  <el-dialog :title="'分配'" :close-on-click-modal="false" :visible.sync="visible">
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmit()"
      label-width="80px"
    >
      <el-form-item>
        <el-select v-model="dataForm.taskReceiverId" placeholder="任务领取人">
          <el-option
            v-for="(item,index) in usersList"
            :label="item.name"
            :value="item.id"
            :key="index"
          ></el-option>
        </el-select>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        taskReceiverId: ""
      },
      userInfo: { id: "", name: "" },
      usersList: userInfo[10],
      dataRule: {
        taskReceiverId: [
          { required: true, message: "任务领取人id不能为空", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    init(ids, operate) {
      this.visible = true;
      this.$nextTick(() => {
        //this.$refs['dataForm'].resetFields()
        if (operate) {
          this.$http({
            url: this.$http.adornUrl("/express/task/getCourier"),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              for (var i = 0; i < data.userList.length; i++) {
                this.userInfo.id = data.userList[i].userId;
                this.userInfo.name = data.userList[i].username;
                this.usersList.push(this.userInfo);
                console.log(this.usersList);
              }
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl("/express/task/allocation"),
            method: "post",
            data: this.$http.adornData({
              taskReceiverId: this.dataForm.taskReceiverId
            })
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message({
                message: "操作成功",
                type: "success",
                duration: 1500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                }
              });
            } else {
              this.$message.error(data.msg);
            }
          });
        }
      });
    }
  }
};
</script>
