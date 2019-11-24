<template>
    <el-button v-if="isAuth('express:qrcode:print')" type="primary" @click="pintQRCode()">打印二维码</el-button>
</template>
<script>
  export default {
    data () {
      return {
        dataForm: {
          key: ''
        },
      }
    },
    
    methods: {
      // 新增 / 修改
      pintQRCode () {
       this.$http({
          url: this.$http.adornUrl('/express/qrcode/print'),
          method: 'get',
          params: this.$http.adornParams({
            'key': "123"
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
              console.log(data);
          } else {
            console.log(data);
          }
          
        })
      },
    }
  }
</script>