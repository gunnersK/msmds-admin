<template>
  <div class="app-container">
    <el-table
      :data="list"
      style="width: 100%"
      height="600px"
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
      <el-table-column label="弹窗名称" width="95">
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
  </div>
</template>

<script>
  import { getList } from '@/api/table'

  export default {
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
        list: [],
        listLoading: true
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
          data: {},
          headers: {'Authorization': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOiI1OTU4NTQ0IiwidHlwZSI6IjAiLCJleHAiOjE2MDYzMDg0NDAsImlhdCI6MTYwMzYzMDA0MH0.kuTjokWy6qN9z3Bh6XlzF-6pM__iJNDPKBh5m2S2SiM'},
          url: 'https://test.msmds.cn/jplus/admin/popWin/list/all'
        }).then((resp) => {
          //用户类型

          this.list = resp.data.data
          // this.list.forEach(item=>{
          //   item.userType = userType[item.userType]
          // })
        })
      }
    }
  }
</script>
