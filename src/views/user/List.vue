<template>
  <div class="search">
    <el-form :inline="true" :model="formInline" class="form-inline">
      <el-form-item label="用户名">
        <el-input v-model="formInline.name" placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="formInline.email" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="tableData" stripe style="width: 100%" :highlight-current-row="true">
      <el-table-column prop="id" label="编号"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column prop="age" label="年龄"></el-table-column>
      <el-table-column prop="status" label="状态" :formatter="formatStatus"></el-table-column>
      <el-table-column prop="email" label="邮箱"></el-table-column>
      <el-table-column prop="mobile" label="电话号码"></el-table-column>
      <el-table-column prop="isAdmin" label="是否管理员" :formatter="formatIsAdmin"></el-table-column>
      <el-table-column prop="info" label="备注信息"></el-table-column>
      <el-table-column fixed="right" label="操作">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
          <el-button type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="page">
      <el-pagination layout="prev, pager, next" :total="total" :background="true"></el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserList',
  data () {
    return {
      formInline: {
        name: '',
        email: ''
      },
      total: 0,
      tableData: [
        { id: 1, name: 'gouduenzi', age: 17, status: 'gg', email: 'jj@s.com', company: '', mobile: '1363636456', isAdmin: 'yes', info: '' }
      ]
    }
  },
  methods: {
    formatIsAdmin (row, column, cellValue) {
      return cellValue ? '是' : '否'
    },
    formatStatus (row, column, cellValue) {
      return cellValue === 'ok' ? '正常' : '已禁用'
    },
    handleClick (row) {
      console.log('当前row数据：', row)
    },
    onSubmit () {
      console.log(this.formInline)
    }
  },
  mounted () {
    this.$http.getUserList().then(res => {
      this.tableData = res.rows
      this.total = this.tableData.length
    }).catch(err => {
      this.$message.error(err.message)
    })
  }
}
</script>

<style scoped>
.search {
  width: 100%;
  height: 100%;
  text-align: left;
}
.page {
  text-align: center;
  margin-top: 20px;
}
</style>
