<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户名" prop="username">
      <el-input v-model="dataForm.username" placeholder="用户名"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="password">
      <el-input v-model="dataForm.password" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item label="手机号" prop="mobile">
      <el-input v-model="dataForm.mobile" placeholder="手机号"></el-input>
    </el-form-item>
    <el-form-item label="用户角色" prop="userRole">
      <el-input v-model="dataForm.userRole" placeholder="用户角色"></el-input>
    </el-form-item>
    <el-form-item label="用户类型（0用户1快递员）" prop="userType">
      <el-input v-model="dataForm.userType" placeholder="用户类型（0用户1快递员）"></el-input>
    </el-form-item>
    <el-form-item label="头像路径" prop="userImg">
      <el-input v-model="dataForm.userImg" placeholder="头像路径"></el-input>
    </el-form-item>
    <el-form-item label="微信id" prop="wechatId">
      <el-input v-model="dataForm.wechatId" placeholder="微信id"></el-input>
    </el-form-item>
    <el-form-item label="微信名" prop="wechatName">
      <el-input v-model="dataForm.wechatName" placeholder="微信名"></el-input>
    </el-form-item>
    <el-form-item label="身份信息号码" prop="idcar">
      <el-input v-model="dataForm.idcar" placeholder="身份信息号码"></el-input>
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
          userId: 0,
          username: '',
          password: '',
          mobile: '',
          userRole: '',
          userType: '',
          userImg: '',
          wechatId: '',
          wechatName: '',
          idcar: '',
          createBy: '',
          createTime: '',
          modifyBy: '',
          modifyTime: ''
        },
        dataRule: {
          username: [
            { required: true, message: '用户名不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' }
          ],
          mobile: [
            { required: true, message: '手机号不能为空', trigger: 'blur' }
          ],
          userRole: [
            { required: true, message: '用户角色不能为空', trigger: 'blur' }
          ],
          userType: [
            { required: true, message: '用户类型（0用户1快递员）不能为空', trigger: 'blur' }
          ],
          userImg: [
            { required: true, message: '头像路径不能为空', trigger: 'blur' }
          ],
          wechatId: [
            { required: true, message: '微信id不能为空', trigger: 'blur' }
          ],
          wechatName: [
            { required: true, message: '微信名不能为空', trigger: 'blur' }
          ],
          idcar: [
            { required: true, message: '身份信息号码不能为空', trigger: 'blur' }
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
      init (id) {
        this.dataForm.userId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.userId) {
            this.$http({
              url: this.$http.adornUrl(`/express/suser/info/${this.dataForm.userId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.username = data.suser.username
                this.dataForm.password = data.suser.password
                this.dataForm.mobile = data.suser.mobile
                this.dataForm.userRole = data.suser.userRole
                this.dataForm.userType = data.suser.userType
                this.dataForm.userImg = data.suser.userImg
                this.dataForm.wechatId = data.suser.wechatId
                this.dataForm.wechatName = data.suser.wechatName
                this.dataForm.idcar = data.suser.idcar
                this.dataForm.createBy = data.suser.createBy
                this.dataForm.createTime = data.suser.createTime
                this.dataForm.modifyBy = data.suser.modifyBy
                this.dataForm.modifyTime = data.suser.modifyTime
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
              url: this.$http.adornUrl(`/express/suser/${!this.dataForm.userId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'userId': this.dataForm.userId || undefined,
                'username': this.dataForm.username,
                'password': this.dataForm.password,
                'mobile': this.dataForm.mobile,
                'userRole': this.dataForm.userRole,
                'userType': this.dataForm.userType,
                'userImg': this.dataForm.userImg,
                'wechatId': this.dataForm.wechatId,
                'wechatName': this.dataForm.wechatName,
                'idcar': this.dataForm.idcar,
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
