<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="快递公司编码" prop="expressCode">
      <el-input v-model="dataForm.expressCode" placeholder="快递公司编码"></el-input>
    </el-form-item>
    <el-form-item label="快递公司名称" prop="expressName">
      <el-input v-model="dataForm.expressName" placeholder="快递公司名称"></el-input>
    </el-form-item>
    <el-form-item label="快递公司头像" prop="expressImg">
      <el-input v-model="dataForm.expressImg" placeholder="快递公司头像"></el-input>
    </el-form-item>
    <el-form-item label="快递公司电话" prop="expressPhone">
      <el-input v-model="dataForm.expressPhone" placeholder="快递公司电话"></el-input>
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
          expressId: 0,
          expressCode: '',
          expressName: '',
          expressImg: '',
          expressPhone: ''
        },
        dataRule: {
          expressCode: [
            { required: true, message: '快递公司编码不能为空', trigger: 'blur' }
          ],
          expressName: [
            { required: true, message: '快递公司名称不能为空', trigger: 'blur' }
          ],
          expressImg: [
            { required: true, message: '快递公司头像不能为空', trigger: 'blur' }
          ],
          expressPhone: [
            { required: true, message: '快递公司电话不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.expressId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.expressId) {
            this.$http({
              url: this.$http.adornUrl(`/express/expresscompany/info/${this.dataForm.expressId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.expressCode = data.expresscompany.expressCode
                this.dataForm.expressName = data.expresscompany.expressName
                this.dataForm.expressImg = data.expresscompany.expressImg
                this.dataForm.expressPhone = data.expresscompany.expressPhone
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
              url: this.$http.adornUrl(`/express/expresscompany/${!this.dataForm.expressId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'expressId': this.dataForm.expressId || undefined,
                'expressCode': this.dataForm.expressCode,
                'expressName': this.dataForm.expressName,
                'expressImg': this.dataForm.expressImg,
                'expressPhone': this.dataForm.expressPhone
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
