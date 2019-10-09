<template>
  <el-dialog
    :title="dataForm.operate ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="任务领取人id" prop="taskReceiverId">
      <el-input v-model="dataForm.taskReceiverId" placeholder="任务领取人id"></el-input>
    </el-form-item>
    <el-form-item label="用户id" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户id"></el-input>
    </el-form-item>
    <el-form-item label="订单id" prop="orderId">
      <el-input v-model="dataForm.orderId" placeholder="订单id"></el-input>
    </el-form-item>
    <el-form-item label="运单号" prop="transportNo">
      <el-input v-model="dataForm.transportNo" placeholder="运单号"></el-input>
    </el-form-item>
    <el-form-item label="发/收手机号" prop="phoneNum">
      <el-input v-model="dataForm.phoneNum" placeholder="发/收手机号"></el-input>
    </el-form-item>
    <el-form-item label="取货/派送地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="取货/派送地址"></el-input>
    </el-form-item>
    <el-form-item label="任务类型（0 揽件；1派送）" prop="taskType">
      <el-input v-model="dataForm.taskType" placeholder="任务类型（0 揽件；1派送）"></el-input>
    </el-form-item>
    <el-form-item label="任务状态（0有效；1取消）" prop="taskStatus">
      <el-input v-model="dataForm.taskStatus" placeholder="任务状态（0有效；1取消）"></el-input>
    </el-form-item>
    <el-form-item label="分配人" prop="allocationBy">
      <el-input v-model="dataForm.allocationBy" placeholder="分配人"></el-input>
    </el-form-item>
    <el-form-item label="分配时间" prop="allocationTime">
      <el-input v-model="dataForm.allocationTime" placeholder="分配时间"></el-input>
    </el-form-item>
    <el-form-item label="创建人" prop="createBy">
      <el-input v-model="dataForm.createBy" placeholder="创建人"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="修改人" prop="modifyBy">
      <el-input v-model="dataForm.modifyBy" placeholder="修改人"></el-input>
    </el-form-item>
    <el-form-item label="修改时间" prop="modifyTime">
      <el-input v-model="dataForm.modifyTime" placeholder="修改时间"></el-input>
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
    data () {
      return {
        visible: false,
        dataForm: {
          operate: true,
          taskId: 0,
          taskReceiverId: '',
          userId: '',
          orderId: '',
          transportNo: '',
          phoneNum: '',
          address: '',
          taskType: '',
          taskStatus: '',
          allocationBy: '',
          allocationTime: '',
          createBy: '',
          createTime: '',
          modifyBy: '',
          modifyTime: ''
        },
        dataRule: {
          taskReceiverId: [
            { required: true, message: '任务领取人id不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '用户id不能为空', trigger: 'blur' }
          ],
          orderId: [
            { required: true, message: '订单id不能为空', trigger: 'blur' }
          ],
          transportNo: [
            { required: true, message: '运单号不能为空', trigger: 'blur' }
          ],
          phoneNum: [
            { required: true, message: '发/收手机号不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '取货/派送地址不能为空', trigger: 'blur' }
          ],
          taskType: [
            { required: true, message: '任务类型（0 揽件；1派送）不能为空', trigger: 'blur' }
          ],
          taskStatus: [
            { required: true, message: '任务状态（0有效；1取消）不能为空', trigger: 'blur' }
          ],
          allocationBy: [
            { required: true, message: '分配人不能为空', trigger: 'blur' }
          ],
          allocationTime: [
            { required: true, message: '分配时间不能为空', trigger: 'blur' }
          ],
          createBy: [
            { required: true, message: '创建人不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          modifyBy: [
            { required: true, message: '修改人不能为空', trigger: 'blur' }
          ],
          modifyTime: [
            { required: true, message: '修改时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id,operate) {
        this.dataForm.operate = operate
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.taskId) {
            this.$http({
              url: this.$http.adornUrl(`/express/task/info/${this.dataForm.taskId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.taskReceiverId = data.task.taskReceiverId
                this.dataForm.userId = data.task.userId
                this.dataForm.orderId = data.task.orderId
                this.dataForm.transportNo = data.task.transportNo
                this.dataForm.phoneNum = data.task.phoneNum
                this.dataForm.address = data.task.address
                this.dataForm.taskType = data.task.taskType
                this.dataForm.taskStatus = data.task.taskStatus
                this.dataForm.allocationBy = data.task.allocationBy
                this.dataForm.allocationTime = data.task.allocationTime
                this.dataForm.createBy = data.task.createBy
                this.dataForm.createTime = data.task.createTime
                this.dataForm.modifyBy = data.task.modifyBy
                this.dataForm.modifyTime = data.task.modifyTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/express/task/${!this.dataForm.taskId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'taskId': this.dataForm.taskId || undefined,
                'taskReceiverId': this.dataForm.taskReceiverId,
                'userId': this.dataForm.userId,
                'orderId': this.dataForm.orderId,
                'transportNo': this.dataForm.transportNo,
                'phoneNum': this.dataForm.phoneNum,
                'address': this.dataForm.address,
                'taskType': this.dataForm.taskType,
                'taskStatus': this.dataForm.taskStatus,
                'allocationBy': this.dataForm.allocationBy,
                'allocationTime': this.dataForm.allocationTime,
                'createBy': this.dataForm.createBy,
                'createTime': this.dataForm.createTime,
                'modifyBy': this.dataForm.modifyBy,
                'modifyTime': this.dataForm.modifyTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
