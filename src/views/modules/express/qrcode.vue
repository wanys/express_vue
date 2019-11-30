<template>
  <div>
      <el-button v-if="isAuth('express:qrcode:print')" type="primary" @click="pintQRCode()">打印二维码</el-button>
  </div>
</template>

<script>
  import { randomRange } from '@/utils'
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
          method: 'post',
          params: this.$http.adornParams({
            'key': "123"
          }),
          // 二进制流文件，一定要设置成blob，默认是json
          responseType: 'blob'

        }).then(res => {
          const link = document.createElement('a')
          const blob = new Blob([res.data], { type: 'application/pdf' })
          link.style.display = 'none'
          link.href = URL.createObjectURL(blob)

          let fileName = randomRange(10);

          console.log(fileName);
          link.setAttribute('download', fileName+".pdf")
          document.body.appendChild(link)
          link.click()
          document.body.removeChild(link)
        })
      },
    }
  }
</script>