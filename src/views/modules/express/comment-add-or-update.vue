<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="快递员ID" prop="messengerId">
      <el-input v-model="dataForm.messengerId" placeholder="快递员ID"></el-input>
    </el-form-item>
    <el-form-item label="服务质量打分" prop="service">
      <el-input v-model="dataForm.service" placeholder="服务质量打分"></el-input>
    </el-form-item>
    <el-form-item label="物流速度打分" prop="speed">
      <el-input v-model="dataForm.speed" placeholder="物流速度打分"></el-input>
    </el-form-item>
    <el-form-item label="物品完好度" prop="goods">
      <el-input v-model="dataForm.goods" placeholder="物品完好度"></el-input>
    </el-form-item>
    <el-form-item label="评价内容" prop="content">
      <el-input v-model="dataForm.content" placeholder="评价内容"></el-input>
    </el-form-item>
    <el-form-item label="综合得分" prop="score">
      <el-input v-model="dataForm.score" placeholder="综合得分"></el-input>
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
          commentId: 0,
          userId: '',
          messengerId: '',
          service: '',
          speed: '',
          goods: '',
          content: '',
          score: '',
          createBy: '',
          createTime: '',
          modifyBy: '',
          modifyTime: ''
        },
        dataRule: {
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          messengerId: [
            { required: true, message: '快递员ID不能为空', trigger: 'blur' }
          ],
          service: [
            { required: true, message: '服务质量打分不能为空', trigger: 'blur' }
          ],
          speed: [
            { required: true, message: '物流速度打分不能为空', trigger: 'blur' }
          ],
          goods: [
            { required: true, message: '物品完好度不能为空', trigger: 'blur' }
          ],
          content: [
            { required: true, message: '评价内容不能为空', trigger: 'blur' }
          ],
          score: [
            { required: true, message: '综合得分不能为空', trigger: 'blur' }
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
        this.dataForm.commentId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.commentId) {
            this.$http({
              url: this.$http.adornUrl(`/express/comment/info/${this.dataForm.commentId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.comment.userId
                this.dataForm.messengerId = data.comment.messengerId
                this.dataForm.service = data.comment.service
                this.dataForm.speed = data.comment.speed
                this.dataForm.goods = data.comment.goods
                this.dataForm.content = data.comment.content
                this.dataForm.score = data.comment.score
                this.dataForm.createBy = data.comment.createBy
                this.dataForm.createTime = data.comment.createTime
                this.dataForm.modifyBy = data.comment.modifyBy
                this.dataForm.modifyTime = data.comment.modifyTime
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
              url: this.$http.adornUrl(`/express/comment/${!this.dataForm.commentId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'commentId': this.dataForm.commentId || undefined,
                'userId': this.dataForm.userId,
                'messengerId': this.dataForm.messengerId,
                'service': this.dataForm.service,
                'speed': this.dataForm.speed,
                'goods': this.dataForm.goods,
                'content': this.dataForm.content,
                'score': this.dataForm.score,
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
