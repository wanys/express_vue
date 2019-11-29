<template>
  <div>
      <el-button v-if="isAuth('express:qrcode:print')" type="primary" @click="pintQRCode()">打印二维码</el-button>
      <a class='download' :href='downloadhttp' download="" title="下载">下载</a>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    data () {
      return {
        downloadhttp : ""
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
              console.log(data.filepath);
              this.downloadhttp=data.filepath;
          } else {
            console.log(data);
          }
          
        })
      },
    }
  }
</script>