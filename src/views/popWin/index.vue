<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input v-model="listQuery.title" placeholder="Title" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
        Add
      </el-button>
    </div>

    <el-table
      :data="datalist.list"
      style="width: 100%"
      height="850px"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >

      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index + 1}}
        </template>
      </el-table-column>
      <el-table-column label="弹窗id" width="95">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>
      <el-table-column label="弹窗名称" width="95" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>
      <el-table-column label="弹窗类型" width="95">
        <template slot-scope="scope">
          {{ scope.row.winType ? "静态":"动态" }}
        </template>
      </el-table-column>
      <el-table-column label="关联动态弹窗" width="150">
        <template slot-scope="scope">
          {{ scope.row.relatedName }}
        </template>
      </el-table-column>
      <el-table-column label="用户类型" width="200" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          <span>  {{ scope.row.userType | filtUser}} </span>

        </template>
      </el-table-column>
      <el-table-column label="弹窗场景" width="150" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          {{ scope.row.scene }}
        </template>
      </el-table-column>
      <el-table-column label="弹窗次数" width="95">
        <template slot-scope="scope">
          {{ scope.row.time }}
        </template>
      </el-table-column>
      <el-table-column label="频率（天）" width="95">
        <template slot-scope="scope">
          {{ scope.row.frequency }}
        </template>
      </el-table-column>
      <el-table-column label="优先级" width="95">
        <template slot-scope="scope">
          {{ scope.row.priority }}
        </template>
      </el-table-column>
      <el-table-column label="弹窗ui（图片）" width="95" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          {{ scope.row.imgUrl }}
        </template>
      </el-table-column>
      <el-table-column label="跳转地址" width="150" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          {{ scope.row.desLink }}
        </template>
      </el-table-column>
      <el-table-column label="前端数据" width="100" :show-overflow-tooltip="true">
        <template slot-scope="scope">
          {{ scope.row.data }}
        </template>
      </el-table-column>
      <el-table-column label="启用状态" width="95">
        <template slot-scope="scope">
          <el-switch
            v-model=scope.row.status
            :active-value="1"
            :inactive-value="0">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="最低可见版本" width="95">
        <template slot-scope="scope">
          {{ scope.row.lowVersion }}
        </template>
      </el-table-column>
      <el-table-column label="最高可见版本" width="95">
        <template slot-scope="scope">
          {{ scope.row.highVersion }}
        </template>
      </el-table-column>
      <el-table-column label="关联用户数量" width="95">
        <template slot-scope="scope">
          {{ scope.row.userAmount }}
        </template>
      </el-table-column>

<!--      <el-table-column label="Author" width="110" align="center">-->
<!--        <template slot-scope="scope">-->
<!--          <span>{{ scope.row.author }}</span>-->
<!--        </template>-->
<!--      </el-table-column>-->
<!--      <el-table-column class-name="status-col" label="Status" width="110" align="center">-->
<!--        <template slot-scope="scope">-->
<!--          <el-tag :type="scope.row.status | statusFilter">{{ scope.row.status }}</el-tag>-->
<!--        </template>-->
<!--      </el-table-column>-->
<!--      <el-table-column align="center" prop="created_at" label="Display_time" width="200">-->
<!--        <template slot-scope="scope">-->
<!--          <i class="el-icon-time" />-->
<!--          <span>{{ scope.row.display_time }}</span>-->
<!--        </template>-->
<!--      </el-table-column>-->
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="fetchData" />

    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form ref="dataForm" :rules="rules" :model="temp" label-position="left" label-width="70px" style="width: 400px; margin-left:50px;">
<!--        <el-form-item label="Type" prop="type">-->
<!--          <el-select v-model="temp.type" class="filter-item" placeholder="Please select">-->
<!--            <el-option v-for="item in calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key" />-->
<!--          </el-select>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="Date" prop="timestamp">-->
<!--          <el-date-picker v-model="temp.timestamp" type="datetime" placeholder="Please pick a date" />-->
<!--        </el-form-item>-->
        <el-form-item label="Title" prop="title">
          <el-input v-model="temp.title" />
        </el-form-item>
<!--        <el-form-item label="Status">-->
<!--          <el-select v-model="temp.status" class="filter-item" placeholder="Please select">-->
<!--            <el-option v-for="item in statusOptions" :key="item" :label="item" :value="item" />-->
<!--          </el-select>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="Imp">-->
<!--          <el-rate v-model="temp.importance" :colors="['#99A9BF', '#F7BA2A', '#FF9900']" :max="3" style="margin-top:8px;" />-->
<!--        </el-form-item>-->
<!--        <el-form-item label="Remark">-->
<!--          <el-input v-model="temp.remark" :autosize="{ minRows: 2, maxRows: 4}" type="textarea" placeholder="Please input" />-->
<!--        </el-form-item>-->
      </el-form>
<!--      <div slot="footer" class="dialog-footer">-->
<!--        <el-button @click="dialogFormVisible = false">-->
<!--          Cancel-->
<!--        </el-button>-->
<!--        <el-button type="primary" @click="dialogStatus==='create'?createData():updateData()">-->
<!--          Confirm-->
<!--        </el-button>-->
<!--      </div>-->
    </el-dialog>

  </div>
</template>

<script>
  // import { getList } from '@/api/table'
  import Pagination from '@/components/Pagination'

  export default {
    components: { Pagination },
    filters: {
      statusFilter(status) {
        const statusMap = {
          published: 'success',
          draft: 'gray',
          deleted: 'danger'
        }
        return statusMap[status]
      },
      filtUser(userType){
        var userTypeList = {
          '0': '所有用户',
          '1': '有0元购资格，且资格有效期在3天之内的用户',
          '2': '购物红包有效期剩3天的用户',
          '3': '前7天下过美团订单，当天未下美团订单的用户',
          '4': '前7天下过饿了么订单，当天未下饿了么订单的用户',
          '5': '可提现金额＜1的用户，近7天未下单',
          '6': '1≤订单量≤4的用户',
          '7': '近1个月有积分兑换的用户',
          '8': '近1个月使用过购物红包的用户',
          '9': '积分值＜500的用户'
        }
        return userTypeList[userType]
      }
    },
    //保存组件内所有数据
    data() {
      return {
        total: 0,
        datalist: {
          list: [],
          listLoading: true
        },
        listQuery: {
          page: 1,
          limit: 10,
          importance: undefined,
          title: undefined,
          type: undefined,
          sort: '+id'
        },
        dialogFormVisible: false,
        dialogStatus: '',
        textMap: {
          update: 'Edit',
          create: 'Create'
        },
        temp: {
          title: ''
        }
      }
    },
    //组件初始化
    mounted() {
      this.fetchData()
    },
    //保存所有方法
    methods: {
      fetchData() {
        this.axios({
          method: 'POST',
          data: {
            'pageNo': this.listQuery.page,
            'pageSize': this.listQuery.limit
          },
          headers: {'Authorization': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOiI1OTU4NTQ0IiwidHlwZSI6IjAiLCJleHAiOjE2MDYzMDg0NDAsImlhdCI6MTYwMzYzMDA0MH0.kuTjokWy6qN9z3Bh6XlzF-6pM__iJNDPKBh5m2S2SiM'},
          url: 'https://test.msmds.cn/jplus/admin/popWin/list/all'
        }).then((resp) => {
          //用户类型

          this.datalist.list = resp.data.data
          this.total = resp.data.total
          // this.list.forEach(item=>{
          //   item.userType = userType[item.userType]
          // })
        })
      },
      handleCreate() {
        this.dialogStatus = 'create'
        this.dialogFormVisible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].clearValidate()
        })
      }
    }
  }
</script>
