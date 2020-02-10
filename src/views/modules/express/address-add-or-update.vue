<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="" prop="id">
      <el-input v-model="dataForm.id" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="地址名字" prop="name">
      <el-input v-model="dataForm.name" placeholder="地址名字"></el-input>
    </el-form-item>
    <el-form-item label="地址电话" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="地址电话"></el-input>
    </el-form-item>
    <el-form-item label="所在省" prop="provence">
      <el-input v-model="dataForm.provence" placeholder="所在省"></el-input>
    </el-form-item>
    <el-form-item label="所在市" prop="city">
      <el-input v-model="dataForm.city" placeholder="所在市"></el-input>
    </el-form-item>
    <el-form-item label="所在区" prop="area">
      <el-input v-model="dataForm.area" placeholder="所在区"></el-input>
    </el-form-item>
    <el-form-item label="详细地址" prop="detailAddr">
      <el-input v-model="dataForm.detailAddr" placeholder="详细地址"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="创建人" prop="createBy">
      <el-input v-model="dataForm.createBy" placeholder="创建人"></el-input>
    </el-form-item>
    <el-form-item label="修改时间" prop="modifyTime">
      <el-input v-model="dataForm.modifyTime" placeholder="修改时间"></el-input>
    </el-form-item>
    <el-form-item label="修改人" prop="modifyBy">
      <el-input v-model="dataForm.modifyBy" placeholder="修改人"></el-input>
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
          id: '',
          userId: 0,
          name: '',
          phone: '',
          provence: '',
          city: '',
          area: '',
          detailAddr: '',
          createTime: '',
          createBy: '',
          modifyTime: '',
          modifyBy: ''
        },
        dataRule: {
          id: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '地址名字不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '地址电话不能为空', trigger: 'blur' }
          ],
          provence: [
            { required: true, message: '所在省不能为空', trigger: 'blur' }
          ],
          city: [
            { required: true, message: '所在市不能为空', trigger: 'blur' }
          ],
          area: [
            { required: true, message: '所在区不能为空', trigger: 'blur' }
          ],
          detailAddr: [
            { required: true, message: '详细地址不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          createBy: [
            { required: true, message: '创建人不能为空', trigger: 'blur' }
          ],
          modifyTime: [
            { required: true, message: '修改时间不能为空', trigger: 'blur' }
          ],
          modifyBy: [
            { required: true, message: '修改人不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.userId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.userId) {
            this.$http({
              url: this.$http.adornUrl(`/express/address/info/${this.dataForm.userId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.id = data.address.id
                this.dataForm.name = data.address.name
                this.dataForm.phone = data.address.phone
                this.dataForm.provence = data.address.provence
                this.dataForm.city = data.address.city
                this.dataForm.area = data.address.area
                this.dataForm.detailAddr = data.address.detailAddr
                this.dataForm.createTime = data.address.createTime
                this.dataForm.createBy = data.address.createBy
                this.dataForm.modifyTime = data.address.modifyTime
                this.dataForm.modifyBy = data.address.modifyBy
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
              url: this.$http.adornUrl(`/express/address/${!this.dataForm.userId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id,
                'userId': this.dataForm.userId || undefined,
                'name': this.dataForm.name,
                'phone': this.dataForm.phone,
                'provence': this.dataForm.provence,
                'city': this.dataForm.city,
                'area': this.dataForm.area,
                'detailAddr': this.dataForm.detailAddr,
                'createTime': this.dataForm.createTime,
                'createBy': this.dataForm.createBy,
                'modifyTime': this.dataForm.modifyTime,
                'modifyBy': this.dataForm.modifyBy
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
