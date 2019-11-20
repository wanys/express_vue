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
            v-for="(item,index) in coueierList"
            :label="item.username"
            :value="item.userId"
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
	
import qs from 'qs'

export default {
  data() {
    return {
      visible: false,
      dataForm: {
        taskReceiverId: ""
      },
      coueierList: [],//快递员列表
      taskIds: [],
      dataRule: {
        taskReceiverId: [
          { required: true, message: "任务领取人id不能为空", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    init(ids, operate) {
      this.taskIds=ids;
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
              this.coueierList=data.courierList;

            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      this.$refs["dataForm"].validate(valid => {
        console.log(this.dataForm.taskReceiverId);
        console.log(this.taskIds);
        let comment={
              taskReceiverId: this.dataForm.taskReceiverId,
              taskIds: this.taskIds}
        if (valid) {
          this.$http({
            url: this.$http.adornUrl("/express/task/allocation"),
            method: "post",
            data: qs.stringify(comment)
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
