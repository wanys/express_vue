<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.taskId" placeholder="任务编号" clearable></el-input>
      </el-form-item>
       <el-form-item>
        <el-input v-model="dataForm.taskReceiverId" placeholder="任务领取人" clearable></el-input>
      </el-form-item>
       <el-form-item>
        <el-input v-model="dataForm.orderId" placeholder="订单ID" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-select v-model="dataForm.taskType" placeholder="任务类型">
          <el-option v-for="(item,index) in dataForm.typeItems" 
          :label="item.label" :value="item.value" :key="index"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select v-model="dataForm.taskStatus" placeholder="任务状态" >
           <el-option v-for="(item,index) in dataForm.statusItems" 
          :label="item.label" :value="item.value" :key="index"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.transportNo" placeholder="运单号" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.phoneNum" placeholder="手机号" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.province" placeholder="省" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.city" placeholder="市" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.area" placeholder="区" clearable></el-input>
      </el-form-item>
      
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('express:task:save')" type="primary" @click="addOrUpdateHandle('')">新增</el-button>
        <el-button v-if="isAuth('express:task:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
        <el-button v-if="isAuth('express:task:allocation')" type="success" @click="allocationHandle()" :disabled="dataListSelections.length <= 0">批量分配</el-button>
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
      <el-table-column
        prop="taskId"
        header-align="center"
        align="center"
        label="任务编号"
        width="170"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="taskReceiverId"
        header-align="center"
        align="center"
        label="任务领取人"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="orderId"
        header-align="center"
        align="center"
        label="订单ID"
        width="170"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="taskType"
        header-align="center"
        align="center"
        label="类型"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="taskStatus"
        header-align="center"
        align="center"
        label="任务状态"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="transportNo"
        header-align="center"
        align="center"
        label="运单号"
        width="170"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="phoneNum"
        header-align="center"
        align="center"
        label="手机号"
        width="120"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="province"
        header-align="center"
        align="center"
        label="省"
        width="100"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="city"
        header-align="center"
        align="center"
        label="市"
        width="100"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="area"
        header-align="center"
        align="center"
        label="区"
        width="100"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="address"
        header-align="center"
        align="center"
        label="地址"
        width="200"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="allocationBy"
        header-align="center"
        align="center"
        label="分配人"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="allocationTime"
        header-align="center"
        align="center"
        label="分配时间"
        width="100"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="createBy"
        header-align="center"
        align="center"
        label="创建人"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        label="创建时间"
        width="170"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="modifyBy"
        header-align="center"
        align="center"
        label="修改人"
        width="100%"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        prop="modifyTime"
        header-align="center"
        align="center"
        label="修改时间"
        width="170"
        show-overflow-tooltip="true">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.taskId)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.taskId)">删除</el-button>
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
    <!-- 弹窗, 新增 / 修改 / 分配 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    <!-- 弹窗, 分配 -->
    <allocation v-if="allocationVisible" ref="allocation" @refreshDataList="getDataList"></allocation>
  </div>
</template>

<script>
  import AddOrUpdate from './task-add-or-update'
  import Allocation from './task-allocation'
  export default {
    data () {
      return {
        dataForm: {
          taskId: '',
          taskReceiverId: '',
          orderId: '',
          taskType: '',
          taskStatus: '10',
          transportNo: '',
          phoneNum: '',
          province: '',
          city: '',
          area: '',
          typeItems: [
            {
              value: '',
              label: '其他'
            },
            {
              value: 'pull',
              label: '揽件'
            },
            {
              value: 'push',
              label: '派件'
            }
          ],
          statusItems: [
            {
              value: '00',
              label: '删除'
            },
            {
              value: '10',
              label: '创建'
            },
            {
              value: '20',
              label: '分配'
            }
          ]
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false,
        allocationVisible: false,
      }
    },
    components: {
      AddOrUpdate,
      Allocation
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/express/task/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'taskId': this.dataForm.taskId,
            'taskReceiverId': this.dataForm.taskReceiverId,
            'orderId': this.dataForm.orderId,
            'taskType': this.dataForm.taskType,
            'taskStatus': this.dataForm.taskStatus,
            'transportNo': this.dataForm.transportNo,
            'phoneNum': this.dataForm.phoneNum,
            'province': this.dataForm.province,
            'city': this.dataForm.city,
            'area': this.dataForm.area,
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
        var operate=false;
        if(id=='')
        {
          operate=true;
        }
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id,operate)
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.taskId
        })
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/express/task/delete'),
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
      },

      //分配
      allocationHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.taskId
        })

        this.allocationVisible = true
        this.$nextTick(() => {
          this.$refs.allocation.init(ids,true)
        })
      }
    }
  }
</script>
