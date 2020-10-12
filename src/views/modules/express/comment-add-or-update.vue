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
    <el-form-item label="订单id" prop="orderId">
      <el-input v-model="dataForm.orderId" placeholder="订单id"></el-input>
    </el-form-item>
    <el-form-item label="快递员服务态度" prop="serviceAttitude">
      <el-input-number v-model="dataForm.serviceAttitude" controls-position="right" :min="1":max="10" label="快递员服务态度"></el-input-number>
    </el-form-item>
    <el-form-item label="快递员专业性" prop="professional">
      <el-input-number v-model="dataForm.professional" controls-position="right" :min="1":max="10" label="快递员专业性"></el-input-number>
    </el-form-item>
    <el-form-item label="费用合理性" prop="spending">
      <el-input-number v-model="dataForm.spending" controls-position="right" :min="1":max="10" label="费用合理性"></el-input-number>
    </el-form-item>
    <el-form-item label="取货/送货时间" prop="getSpeed">
      <el-input v-model="dataForm.getSpeed" placeholder="取货/送货时间"></el-input>
    </el-form-item>
    <el-form-item label="发货/派送时间" prop="beginSpeed">
      <el-input v-model="dataForm.beginSpeed" placeholder="发货/派送时间"></el-input>
    </el-form-item>
    <el-form-item label="物品运输速度" prop="transportSpeed">
      <el-input-number v-model="dataForm.transportSpeed" controls-position="right" :min="1":max="10" label="物品运输速度"></el-input-number>
    </el-form-item>
    <el-form-item label="个人信息安全" prop="personalInfoSec">
      <el-input-number v-model="dataForm.personalInfoSec" controls-position="right" :min="1":max="10" label="个人信息安全"></el-input-number>
    </el-form-item>
    <el-form-item label="物品完整度" prop="complete">
      <el-input-number v-model="dataForm.complete" controls-position="right" :min="1":max="10" label="物品完整度"></el-input-number>
    </el-form-item>
    <el-form-item label="物品安全性" prop="goodsSec">
      <el-input-number v-model="dataForm.goodsSec" controls-position="right" :min="1":max="10" label="物品安全性"></el-input-number>
    </el-form-item>
    <el-form-item label="评价内容" prop="content">
      <el-input v-model="dataForm.content" placeholder="评价内容"></el-input>
    </el-form-item>
    <el-form-item label="综合得分" prop="score">
      <el-input-number v-model="dataForm.score" controls-position="right" :min="1":max="10" label="综合得分"></el-input-number>
    </el-form-item>
    <!--<el-form-item label="创建人" prop="createBy">
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
    </el-form-item>-->
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
          orderId: '',
          serviceAttitude: '',
          professional: '',
          spending: '',
          getSpeed: '',
          beginSpeed: '',
          transportSpeed: '',
          personalInfoSec: '',
          complete: '',
          goodsSec: '',
          content: '',
          score: ''
          /* createBy: '',
          createTime: '',
          modifyBy: '',
          modifyTime: '' */
        },
        dataRule: {
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          messengerId: [
            { required: true, message: '快递员ID不能为空', trigger: 'blur' }
          ],
          orderId: [
            { required: true, message: '订单id不能为空', trigger: 'blur' }
          ],
          serviceAttitude: [
            { required: true, message: '快递员服务态度不能为空', trigger: 'blur' }
          ],
          professional: [
            { required: true, message: '快递员专业性不能为空', trigger: 'blur' }
          ],
          spending: [
            { required: true, message: '费用合理性不能为空', trigger: 'blur' }
          ],
          getSpeed: [
            { required: true, message: '取货/送货时间不能为空', trigger: 'blur' }
          ],
          beginSpeed: [
            { required: true, message: '下单之后发货时间/网点派送时间不能为空', trigger: 'blur' }
          ],
          transportSpeed: [
            { required: true, message: '物品运输速度不能为空', trigger: 'blur' }
          ],
          personalInfoSec: [
            { required: true, message: '个人信息安全不能为空', trigger: 'blur' }
          ],
          complete: [
            { required: true, message: '物品完整度不能为空', trigger: 'blur' }
          ],
          goodsSec: [
            { required: true, message: '物品安全性不能为空', trigger: 'blur' }
          ],
          content: [
            { required: true, message: '评价内容不能为空', trigger: 'blur' }
          ],
          score: [
            { required: true, message: '综合得分不能为空', trigger: 'blur' }
          ]
          /* createBy: [
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
          ] */
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
                this.dataForm.orderId = data.comment.orderId
                this.dataForm.serviceAttitude = data.comment.serviceAttitude
                this.dataForm.professional = data.comment.professional
                this.dataForm.spending = data.comment.spending
                this.dataForm.getSpeed = data.comment.getSpeed
                this.dataForm.beginSpeed = data.comment.beginSpeed
                this.dataForm.transportSpeed = data.comment.transportSpeed
                this.dataForm.personalInfoSec = data.comment.personalInfoSec
                this.dataForm.complete = data.comment.complete
                this.dataForm.goodsSec = data.comment.goodsSec
                this.dataForm.content = data.comment.content
                this.dataForm.score = data.comment.score
                /* this.dataForm.createBy = data.comment.createBy
                this.dataForm.createTime = data.comment.createTime
                this.dataForm.modifyBy = data.comment.modifyBy
                this.dataForm.modifyTime = data.comment.modifyTime */
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
                'orderId': this.dataForm.orderId,
                'serviceAttitude': this.dataForm.serviceAttitude,
                'professional': this.dataForm.professional,
                'spending': this.dataForm.spending,
                'getSpeed': this.dataForm.getSpeed,
                'beginSpeed': this.dataForm.beginSpeed,
                'transportSpeed': this.dataForm.transportSpeed,
                'personalInfoSec': this.dataForm.personalInfoSec,
                'complete': this.dataForm.complete,
                'goodsSec': this.dataForm.goodsSec,
                'content': this.dataForm.content,
                'score': this.dataForm.score
                /* 'createBy': this.dataForm.createBy,
                'createTime': this.dataForm.createTime,
                'modifyBy': this.dataForm.modifyBy,
                'modifyTime': this.dataForm.modifyTime */
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
