<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.content" placeholder="评论内容" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('express:comment:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('express:comment:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <!--<el-table-column
        prop="commentId"
        header-align="center"
        align="center"
        label="ID">
      </el-table-column>-->
      <el-table-column
        prop="userId"
        header-align="center"
        align="center"
        label="用户">
      </el-table-column>
      <el-table-column
        prop="messengerId"
        header-align="center"
        align="center"
        label="快递员">
      </el-table-column>
      <el-table-column
        prop="orderId"
        header-align="center"
        align="center"
        label="订单号">
      </el-table-column>
      <el-table-column
        prop="serviceAttitude"
        header-align="center"
        align="center"
        label="快递员服务态度">
      </el-table-column>
      <el-table-column
        prop="professional"
        header-align="center"
        align="center"
        label="快递员专业性">
      </el-table-column>
      <el-table-column
        prop="spending"
        header-align="center"
        align="center"
        label="费用合理性">
      </el-table-column>
      <el-table-column
        prop="getSpeed"
        header-align="center"
        align="center"
        label="取货/送货时间">
      </el-table-column>
      <el-table-column
        prop="beginSpeed"
        header-align="center"
        align="center"
        label="发货/派送时间">
      </el-table-column>
      <el-table-column
        prop="transportSpeed"
        header-align="center"
        align="center"
        label="物品运输速度">
      </el-table-column>
      <el-table-column
        prop="personalInfoSec"
        header-align="center"
        align="center"
        label="个人信息安全">
      </el-table-column>
      <el-table-column
        prop="complete"
        header-align="center"
        align="center"
        label="物品完整度">
      </el-table-column>
      <el-table-column
        prop="goodsSec"
        header-align="center"
        align="center"
        label="物品安全性">
      </el-table-column>
      <el-table-column
        prop="content"
        header-align="center"
        align="center"
        label="评价内容">
      </el-table-column>
      <el-table-column
        prop="score"
        header-align="center"
        align="center"
        label="综合得分">
      </el-table-column>
      <!--<el-table-column
        prop="createBy"
        header-align="center"
        align="center"
        label="创建人">
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        label="创建时间">
      </el-table-column>
      <el-table-column
        prop="modifyBy"
        header-align="center"
        align="center"
        label="修改人">
      </el-table-column>
      <el-table-column
        prop="modifyTime"
        header-align="center"
        align="center"
        label="修改时间">
      </el-table-column>-->
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.commentId)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.commentId)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
  import AddOrUpdate from './comment-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          content: ''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/express/comment/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'content': this.dataForm.content
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.commentId
        })
        this.$confirm(`确定对[评论：${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/express/comment/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      }
    }
  }
</script>
