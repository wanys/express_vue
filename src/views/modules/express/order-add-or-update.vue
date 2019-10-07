<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <!-- <el-form-item label="订单编号" prop="orderId">
      <el-input v-model="dataForm.orderId" placeholder="订单编号"></el-input>
    </el-form-item> -->
    <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="寄件人姓名" prop="senderName">
      <el-input v-model="dataForm.senderName" placeholder="寄件人姓名"></el-input>
    </el-form-item>
    <el-form-item label="寄件人号码" prop="senderPhone">
      <el-input v-model="dataForm.senderPhone" placeholder="寄件人号码"></el-input>
    </el-form-item>
    <el-form-item label="寄件人地址" prop="senderAddr">
      <el-input v-model="dataForm.senderAddr" placeholder="寄件人地址"></el-input>
    </el-form-item>
    <el-form-item label="收件人姓名" prop="receiverName">
      <el-input v-model="dataForm.receiverName" placeholder="收件人姓名"></el-input>
    </el-form-item>
    <el-form-item label="收件人号码" prop="receiverPhone">
      <el-input v-model="dataForm.receiverPhone" placeholder="收件人号码"></el-input>
    </el-form-item>
    <el-form-item label="收件人地址" prop="receiverAddr">
      <el-input v-model="dataForm.receiverAddr" placeholder="收件人地址"></el-input>
    </el-form-item>
    <el-form-item label="物品种类" prop="goodsType">
      <el-input v-model="dataForm.goodsType" placeholder="物品种类"></el-input>
    </el-form-item>
    <el-form-item label="运单号" prop="transportNo">
      <el-input v-model="dataForm.transportNo" placeholder="运单号"></el-input>
    </el-form-item>
    <el-form-item label="订单状态" prop="orderStatus">
      <el-input v-model="dataForm.orderStatus" placeholder="订单状态"></el-input>
    </el-form-item>
    <!-- <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item> -->
    <el-form-item label="快递公司" prop="express">
      <el-input v-model="dataForm.express" placeholder="快递公司"></el-input>
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
          id: 0,
          orderId: '',
          userId: '',
          senderName: '',
          senderPhone: '',
          senderAddr: '',
          receiverName: '',
          receiverPhone: '',
          receiverAddr: '',
          goodsType: '',
          transportNo: '',
          orderStatus: '',
          // createTime: '',
          express: ''
        },
        dataRule: {
          // orderId: [
          //   { required: true, message: '订单编号不能为空', trigger: 'blur' }
          // ],
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          senderName: [
            { required: true, message: '寄件人姓名不能为空', trigger: 'blur' }
          ],
          senderPhone: [
            { required: true, message: '寄件人号码不能为空', trigger: 'blur' }
          ],
          senderAddr: [
            { required: true, message: '寄件人地址不能为空', trigger: 'blur' }
          ],
          receiverName: [
            { required: true, message: '收件人姓名不能为空', trigger: 'blur' }
          ],
          receiverPhone: [
            { required: true, message: '收件人号码不能为空', trigger: 'blur' }
          ],
          receiverAddr: [
            { required: true, message: '收件人地址不能为空', trigger: 'blur' }
          ],
          goodsType: [
            { required: true, message: '物品种类不能为空', trigger: 'blur' }
          ],
          transportNo: [
            { required: true, message: '运单号不能为空', trigger: 'blur' }
          ],
          orderStatus: [
            { required: true, message: '订单状态不能为空', trigger: 'blur' }
          ],
          // createTime: [
          //   { required: true, message: '创建时间不能为空', trigger: 'blur' }
          // ],
          express: [
            { required: true, message: '快递公司不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/express/order/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.orderId = data.order.orderId
                this.dataForm.userId = data.order.userId
                this.dataForm.senderName = data.order.senderName
                this.dataForm.senderPhone = data.order.senderPhone
                this.dataForm.senderAddr = data.order.senderAddr
                this.dataForm.receiverName = data.order.receiverName
                this.dataForm.receiverPhone = data.order.receiverPhone
                this.dataForm.receiverAddr = data.order.receiverAddr
                this.dataForm.goodsType = data.order.goodsType
                this.dataForm.transportNo = data.order.transportNo
                this.dataForm.orderStatus = data.order.orderStatus
                // this.dataForm.createTime = data.order.createTime
                this.dataForm.express = data.order.express
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
              url: this.$http.adornUrl(`/express/order/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'orderId': this.dataForm.orderId,
                'userId': this.dataForm.userId,
                'senderName': this.dataForm.senderName,
                'senderPhone': this.dataForm.senderPhone,
                'senderAddr': this.dataForm.senderAddr,
                'receiverName': this.dataForm.receiverName,
                'receiverPhone': this.dataForm.receiverPhone,
                'receiverAddr': this.dataForm.receiverAddr,
                'goodsType': this.dataForm.goodsType,
                'transportNo': this.dataForm.transportNo,
                'orderStatus': this.dataForm.orderStatus,
                'createTime': this.dataForm.createTime,
                'express': this.dataForm.express
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
